# Downloading a Vault Inventory in Amazon S3 Glacier Using the AWS SDK for Java<a name="retrieving-vault-inventory-java"></a>

The following are the steps to retrieve a vault inventory using the low\-level API of the AWS SDK for Java\. The high\-level API does not support retrieving a vault inventory\. 

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

    You need to specify an AWS Region where the vault resides\. All operations you perform using this client apply to that AWS Region\. 

1.  Initiate an inventory retrieval job by executing the `initiateJob` method\.

   Run `initiateJob` by providing job information in an `InitiateJobRequest` object\. 
**Note**  
Note that if an inventory has not been completed for the vault an error is returned\. Amazon S3 Glacier \(S3 Glacier\) prepares an inventory for each vault periodically, every 24 hours\. 

   S3 Glacier returns a job ID in response\. The response is available in an instance of the `InitiateJobResult` class\.

   ```
   InitiateJobRequest initJobRequest = new InitiateJobRequest()
       .withVaultName("*** provide vault name ***")
       .withJobParameters(
               new JobParameters()
                   .withType("inventory-retrieval")
                   .withSNSTopic("*** provide SNS topic ARN ****")
         );
   
   InitiateJobResult initJobResult = client.initiateJob(initJobRequest);
   String jobId = initJobResult.getJobId();
   ```

1. Wait for the job to complete\.

   You must wait until the job output is ready for you to download\. If you have either set a notification configuration on the vault, or specified an Amazon Simple Notification Service \(Amazon SNS\) topic when you initiated the job, S3 Glacier sends a message to the topic after it completes the job\. 

   You can also poll S3 Glacier by calling the `describeJob` method to determine job completion status\. However, using an Amazon SNS topic for notification is the recommended approach\. The code example given in the following section uses Amazon SNS for S3 Glacier to publish a message\.

     

1. Download the job output \(vault inventory data\) by executing the `getJobOutput` method\.

   You provide your account ID, job ID, and vault name by creating an instance of the `GetJobOutputRequest` class\. If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is used\. For more information, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\. 

   The output that S3 Glacier returns is available in the `GetJobOutputResult` object\. 

   ```
   GetJobOutputRequest jobOutputRequest = new GetJobOutputRequest()
           .withVaultName("*** provide vault name ***")
           .withJobId("*** provide job ID ***");
   GetJobOutputResult jobOutputResult = client.getJobOutput(jobOutputRequest);
   // jobOutputResult.getBody(); provides the output stream.
   ```

**Note**  
For information about the job related underlying REST API, see [Job Operations](job-operations.md)\.

## Example: Retrieving a Vault Inventory Using the AWS SDK for Java<a name="retrieving-vault-inventory-java-example"></a>

The following Java code example retrieves the vault inventory for the specified vault\.

The example performs the following tasks: 
+ Creates an Amazon Simple Notification Service \(Amazon SNS\) topic\.

  S3 Glacier sends notification to this topic after it completes the job\. 
+ Creates an Amazon Simple Queue Service \(Amazon SQS\) queue\.

  The example attaches a policy to the queue to enable the Amazon SNS topic to post messages to the queue\.
+ Initiates a job to download the specified archive\.

  In the job request, the Amazon SNS topic that was created is specified so that S3 Glacier can publish a notification to the topic after it completes the job\.
+ Checks the Amazon SQS queue for a message that contains the job ID\.

  If there is a message, parse the JSON and check if the job completed successfully\. If it did, download the archive\. 
+ Cleans up by deleting the Amazon SNS topic and the Amazon SQS queue that it created\.

```
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import com.fasterxml.jackson.core.JsonFactory;
import com.fasterxml.jackson.core.JsonParseException;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import com.amazonaws.AmazonClientException;
import com.amazonaws.auth.policy.Policy;
import com.amazonaws.auth.policy.Principal;
import com.amazonaws.auth.policy.Resource;
import com.amazonaws.auth.policy.Statement;
import com.amazonaws.auth.policy.Statement.Effect;
import com.amazonaws.auth.policy.actions.SQSActions;
import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.model.GetJobOutputRequest;
import com.amazonaws.services.glacier.model.GetJobOutputResult;
import com.amazonaws.services.glacier.model.InitiateJobRequest;
import com.amazonaws.services.glacier.model.InitiateJobResult;
import com.amazonaws.services.glacier.model.JobParameters;
import com.amazonaws.services.sns.AmazonSNSClient;
import com.amazonaws.services.sns.model.CreateTopicRequest;
import com.amazonaws.services.sns.model.CreateTopicResult;
import com.amazonaws.services.sns.model.DeleteTopicRequest;
import com.amazonaws.services.sns.model.SubscribeRequest;
import com.amazonaws.services.sns.model.SubscribeResult;
import com.amazonaws.services.sns.model.UnsubscribeRequest;
import com.amazonaws.services.sqs.AmazonSQSClient;
import com.amazonaws.services.sqs.model.CreateQueueRequest;
import com.amazonaws.services.sqs.model.CreateQueueResult;
import com.amazonaws.services.sqs.model.DeleteQueueRequest;
import com.amazonaws.services.sqs.model.GetQueueAttributesRequest;
import com.amazonaws.services.sqs.model.GetQueueAttributesResult;
import com.amazonaws.services.sqs.model.Message;
import com.amazonaws.services.sqs.model.ReceiveMessageRequest;
import com.amazonaws.services.sqs.model.SetQueueAttributesRequest;


public class AmazonGlacierDownloadInventoryWithSQSPolling {

    public static String vaultName = "*** provide vault name ***";
    public static String snsTopicName = "*** provide topic name ***";
    public static String sqsQueueName = "*** provide queue name ***";
    public static String sqsQueueARN;
    public static String sqsQueueURL;
    public static String snsTopicARN;
    public static String snsSubscriptionARN;
    public static String fileName = "*** provide file name ***";
    public static String region = "*** region ***";
    public static long sleepTime = 600; 
    public static AmazonGlacierClient client;
    public static AmazonSQSClient sqsClient;
    public static AmazonSNSClient snsClient;
    
    public static void main(String[] args) throws IOException {
        
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier." + region + ".amazonaws.com");
        sqsClient = new AmazonSQSClient(credentials);
        sqsClient.setEndpoint("https://sqs." + region + ".amazonaws.com");
        snsClient = new AmazonSNSClient(credentials);
        snsClient.setEndpoint("https://sns." + region + ".amazonaws.com");
        
        try {
            setupSQS();
            
            setupSNS();

            String jobId = initiateJobRequest();
            System.out.println("Jobid = " + jobId);
            
            Boolean success = waitForJobToComplete(jobId, sqsQueueURL);
            if (!success) { throw new Exception("Job did not complete successfully."); }
            
            downloadJobOutput(jobId);
            
            cleanUp();
            
        } catch (Exception e) {
            System.err.println("Inventory retrieval failed.");
            System.err.println(e);
        }   
    }

    private static void setupSQS() {
        CreateQueueRequest request = new CreateQueueRequest()
            .withQueueName(sqsQueueName);
        CreateQueueResult result = sqsClient.createQueue(request);  
        sqsQueueURL = result.getQueueUrl();
                
        GetQueueAttributesRequest qRequest = new GetQueueAttributesRequest()
            .withQueueUrl(sqsQueueURL)
            .withAttributeNames("QueueArn");
        
        GetQueueAttributesResult qResult = sqsClient.getQueueAttributes(qRequest);
        sqsQueueARN = qResult.getAttributes().get("QueueArn");
        
        Policy sqsPolicy = 
            new Policy().withStatements(
                    new Statement(Effect.Allow)
                    .withPrincipals(Principal.AllUsers)
                    .withActions(SQSActions.SendMessage)
                    .withResources(new Resource(sqsQueueARN)));
        Map<String, String> queueAttributes = new HashMap<String, String>();
        queueAttributes.put("Policy", sqsPolicy.toJson());
        sqsClient.setQueueAttributes(new SetQueueAttributesRequest(sqsQueueURL, queueAttributes)); 

    }
    private static void setupSNS() {
        CreateTopicRequest request = new CreateTopicRequest()
            .withName(snsTopicName);
        CreateTopicResult result = snsClient.createTopic(request);
        snsTopicARN = result.getTopicArn();

        SubscribeRequest request2 = new SubscribeRequest()
            .withTopicArn(snsTopicARN)
            .withEndpoint(sqsQueueARN)
            .withProtocol("sqs");
        SubscribeResult result2 = snsClient.subscribe(request2);
                
        snsSubscriptionARN = result2.getSubscriptionArn();
    }
    private static String initiateJobRequest() {
        
        JobParameters jobParameters = new JobParameters()
            .withType("inventory-retrieval")
            .withSNSTopic(snsTopicARN);
        
        InitiateJobRequest request = new InitiateJobRequest()
            .withVaultName(vaultName)
            .withJobParameters(jobParameters);
        
        InitiateJobResult response = client.initiateJob(request);
        
        return response.getJobId();
    }
    
    private static Boolean waitForJobToComplete(String jobId, String sqsQueueUrl) throws InterruptedException, JsonParseException, IOException {
        
        Boolean messageFound = false;
        Boolean jobSuccessful = false;
        ObjectMapper mapper = new ObjectMapper();
        JsonFactory factory = mapper.getFactory();
        
        while (!messageFound) {
            List<Message> msgs = sqsClient.receiveMessage(
               new ReceiveMessageRequest(sqsQueueUrl).withMaxNumberOfMessages(10)).getMessages();

            if (msgs.size() > 0) {
                for (Message m : msgs) {
                    JsonParser jpMessage = factory.createJsonParser(m.getBody());
                    JsonNode jobMessageNode = mapper.readTree(jpMessage);
                    String jobMessage = jobMessageNode.get("Message").textValue();
                    
                    JsonParser jpDesc = factory.createJsonParser(jobMessage);
                    JsonNode jobDescNode = mapper.readTree(jpDesc);
                    String retrievedJobId = jobDescNode.get("JobId").textValue();
                    String statusCode = jobDescNode.get("StatusCode").textValue();
                    if (retrievedJobId.equals(jobId)) {
                        messageFound = true;
                        if (statusCode.equals("Succeeded")) {
                            jobSuccessful = true;
                        }
                    }
                }
                
            } else {
              Thread.sleep(sleepTime * 1000); 
            }
          }
        return (messageFound && jobSuccessful);
    }
    
    private static void downloadJobOutput(String jobId) throws IOException {
        
        GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
            .withVaultName(vaultName)
            .withJobId(jobId);
        GetJobOutputResult getJobOutputResult = client.getJobOutput(getJobOutputRequest);
    
        FileWriter fstream = new FileWriter(fileName);
        BufferedWriter out = new BufferedWriter(fstream);
        BufferedReader in = new BufferedReader(new InputStreamReader(getJobOutputResult.getBody()));            
        String inputLine;
        try {
            while ((inputLine = in.readLine()) != null) {
                out.write(inputLine);
            }
        }catch(IOException e) {
            throw new AmazonClientException("Unable to save archive", e);
        }finally{
            try {in.close();}  catch (Exception e) {}
            try {out.close();}  catch (Exception e) {}             
        }
        System.out.println("Retrieved inventory to " + fileName);
    }
    
    private static void cleanUp() {
        snsClient.unsubscribe(new UnsubscribeRequest(snsSubscriptionARN));
        snsClient.deleteTopic(new DeleteTopicRequest(snsTopicARN));
        sqsClient.deleteQueue(new DeleteQueueRequest(sqsQueueURL));
    }
}
```