# Downloading an Archive in Amazon S3 Glacier Using the AWS SDK for Java<a name="downloading-an-archive-using-java"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for Java provide a method to download an archive\.

**Topics**
+ [Downloading an Archive Using the High\-Level API of the AWS SDK for Java](#downloading-an-archive-using-java-highlevel-api)
+ [Downloading an Archive Using the Low\-Level API of the AWS SDK for Java](#downloading-an-archive-using-java-lowlevel-api)

## Downloading an Archive Using the High\-Level API of the AWS SDK for Java<a name="downloading-an-archive-using-java-highlevel-api"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `download` method you can use to download an archive\. 

**Important**  
The `ArchiveTransferManager` class creates an Amazon Simple Notification Service \(Amazon SNS\) topic, and an Amazon Simple Queue Service \(Amazon SQS\) queue that is subscribed to that topic\. It then initiates the archive retrieval job and polls the queue for the archive to be available\. When the archive is available, download begins\. For information about retrieval times, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)\.

### Example: Downloading an Archive Using the High\-Level API of the AWS SDK for Java<a name="download-archives-java-highlevel-example"></a>

The following Java code example downloads an archive from a vault \(`examplevault`\) in the US West \(Oregon\) Region \(`us-west-2`\)\.

For step\-by\-step instructions to run this sample, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with an existing archive ID and the local file path where you want to save the downloaded archive\.

**Example**  

```
import java.io.File;
import java.io.IOException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.transfer.ArchiveTransferManager;
import com.amazonaws.services.sns.AmazonSNSClient;
import com.amazonaws.services.sqs.AmazonSQSClient;


public class ArchiveDownloadHighLevel {
    public static String vaultName = "examplevault";
    public static String archiveId = "*** provide archive ID ***";
    public static String downloadFilePath  = "*** provide location to download archive ***";
    
    public static AmazonGlacierClient glacierClient;
    public static AmazonSQSClient sqsClient;
    public static AmazonSNSClient snsClient;
    
    public static void main(String[] args) throws IOException {
        
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();
        
        glacierClient = new AmazonGlacierClient(credentials);
        
        sqsClient = new AmazonSQSClient(credentials);
        snsClient = new AmazonSNSClient(credentials);
        glacierClient.setEndpoint("glacier.us-west-2.amazonaws.com");
        sqsClient.setEndpoint("sqs.us-west-2.amazonaws.com");
        snsClient.setEndpoint("sns.us-west-2.amazonaws.com");

        try {
            ArchiveTransferManager atm = new ArchiveTransferManager(glacierClient, sqsClient, snsClient);
            
            atm.download(vaultName, archiveId, new File(downloadFilePath));
            System.out.println("Downloaded file to " + downloadFilePath);
            
        } catch (Exception e)
        {
            System.err.println(e);
        }
    }
}
```

## Downloading an Archive Using the Low\-Level API of the AWS SDK for Java<a name="downloading-an-archive-using-java-lowlevel-api"></a>

The following are the steps to retrieve a vault inventory using the AWS SDK for Java low\-level API\. 

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region from where you want to download the archive\. All operations you perform using this client apply to that AWS Region\. 

1. Initiate an `archive-retrieval` job by executing the `initiateJob` method\.

   You provide job information, such as the archive ID of the archive you want to download and the optional Amazon SNS topic to which you want Amazon S3 Glacier \(S3 Glacier\) to post a job completion message, by creating an instance of the `InitiateJobRequest` class\. S3 Glacier returns a job ID in response\. The response is available in an instance of the `InitiateJobResult` class\.

   ```
   JobParameters jobParameters = new JobParameters()
       .withArchiveId("*** provide an archive id ***")
       .withDescription("archive retrieval")
       .withRetrievalByteRange("*** provide a retrieval range***") // optional
       .withType("archive-retrieval");
   
   InitiateJobResult initiateJobResult = client.initiateJob(new InitiateJobRequest()
       .withJobParameters(jobParameters)
       .withVaultName(vaultName));  
             
   String jobId = initiateJobResult.getJobId();
   ```

   You can optionally specify a byte range to request S3 Glacier to prepare only a portion of the archive\. For example, you can update the preceding request by adding the following statement to request S3 Glacier to prepare only the 1 MB to 2 MB portion of the archive\.

   ```
   int ONE_MEG = 1048576;
   String retrievalByteRange = String.format("%s-%s", ONE_MEG, 2*ONE_MEG -1);
   
   JobParameters jobParameters = new JobParameters()
       .withType("archive-retrieval")
       .withArchiveId(archiveId)
       .withRetrievalByteRange(retrievalByteRange) 
       .withSNSTopic(snsTopicARN);
   
   InitiateJobResult initiateJobResult = client.initiateJob(new InitiateJobRequest()
       .withJobParameters(jobParameters)
       .withVaultName(vaultName));  
             
   String jobId = initiateJobResult.getJobId();
   ```

     

1. Wait for the job to complete\.

   You must wait until the job output is ready for you to download\. If you have either set a notification configuration on the vault identifying an Amazon Simple Notification Service \(Amazon SNS\) topic or specified an Amazon SNS topic when you initiated a job, S3 Glacier sends a message to that topic after it completes the job\. 

   You can also poll S3 Glacier by calling the `describeJob` method to determine the job completion status\. Although, using an Amazon SNS topic for notification is the recommended approach\.

1. Download the job output \(archive data\) by executing the `getJobOutput` method\.

   You provide the request information such as the job ID and vault name by creating an instance of the `GetJobOutputRequest` class\. The output that S3 Glacier returns is available in the `GetJobOutputResult` object\. 

   ```
   GetJobOutputRequest jobOutputRequest = new GetJobOutputRequest()
           .withJobId("*** provide a job ID ***")
           .withVaultName("*** provide a vault name ****");
   GetJobOutputResult jobOutputResult = client.getJobOutput(jobOutputRequest);
   
   // jobOutputResult.getBody() // Provides the input stream.
   ```

   The preceding code snippet downloads the entire job output\. You can optionally retrieve only a portion of the output, or download the entire output in smaller chunks by specifying the byte range in your `GetJobOutputRequest`\. 

   ```
   GetJobOutputRequest jobOutputRequest = new GetJobOutputRequest()
           .withJobId("*** provide a job ID ***")
           .withRange("bytes=0-1048575")   // Download only the first 1 MB of the output.
           .withVaultName("*** provide a vault name ****");
   ```

   In response to your `GetJobOutput` call, S3 Glacier returns the checksum of the portion of the data you downloaded, if certain conditions are met\. For more information, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\.

   To verify there are no errors in the download, you can then compute the checksum on the client\-side and compare it with the checksum S3 Glacier sent in the response\. 

   For an archive retrieval job with the optional range specified, when you get the job description, it includes the checksum of the range you are retrieving \(SHA256TreeHash\)\. You can use this value to further verify the accuracy of the entire byte range that you later download\. For example, if you initiate a job to retrieve a tree\-hash aligned archive range and then download output in chunks such that each of your `GetJobOutput` requests return a checksum, then you can compute checksum of each portion you download on the client\-side and then compute the tree hash\. You can compare it with the checksum S3 Glacier returns in response to your describe job request to verify that the entire byte range you have downloaded is the same as the byte range that is stored in S3 Glacier\. 

    For a working example, see [Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for Java—Download Output in Chunks ](#downloading-an-archive-with-range-using-java-example)\. 

### Example 1: Retrieving an Archive Using the Low\-Level API of the AWS SDK for Java<a name="downloading-an-archive-using-java-example"></a>

The following Java code example downloads an archive from the specified vault\. After the job completes, the example downloads the entire output in a single `getJobOutput` call\. For an example of downloading output in chunks, see [Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for Java—Download Output in Chunks ](#downloading-an-archive-with-range-using-java-example)\.

The example performs the following tasks:
+ Creates an Amazon Simple Notification Service \(Amazon SNS\) topic\.

  S3 Glacier sends a notification to this topic after it completes the job\. 
+ Creates an Amazon Simple Queue Service \(Amazon SQS\) queue\.

  The example attaches a policy to the queue to enable the Amazon SNS topic to post messages to the queue\.
+ Initiates a job to download the specified archive\.

  In the job request, the Amazon SNS topic that was created is specified so that S3 Glacier can publish a notification to the topic after it completes the job\.
+ Periodically checks the Amazon SQS queue for a message that contains the job ID\.

  If there is a message, parse the JSON and check if the job completed successfully\. If it did, download the archive\. 
+ Cleans up by deleting the Amazon SNS topic and the Amazon SQS queue that it created\.

```
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.codehaus.jackson.JsonFactory;
import org.codehaus.jackson.JsonNode;
import org.codehaus.jackson.JsonParseException;
import org.codehaus.jackson.JsonParser;
import org.codehaus.jackson.map.ObjectMapper;

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


public class AmazonGlacierDownloadArchiveWithSQSPolling {
    
    public static String archiveId = "*** provide archive ID ****";
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
            System.err.println("Archive retrieval failed.");
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
            .withType("archive-retrieval")
            .withArchiveId(archiveId)
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
        JsonFactory factory = mapper.getJsonFactory();
        
        while (!messageFound) {
            List<Message> msgs = sqsClient.receiveMessage(
               new ReceiveMessageRequest(sqsQueueUrl).withMaxNumberOfMessages(10)).getMessages();

            if (msgs.size() > 0) {
                for (Message m : msgs) {
                    JsonParser jpMessage = factory.createJsonParser(m.getBody());
                    JsonNode jobMessageNode = mapper.readTree(jpMessage);
                    String jobMessage = jobMessageNode.get("Message").getTextValue();
                    
                    JsonParser jpDesc = factory.createJsonParser(jobMessage);
                    JsonNode jobDescNode = mapper.readTree(jpDesc);
                    String retrievedJobId = jobDescNode.get("JobId").getTextValue();
                    String statusCode = jobDescNode.get("StatusCode").getTextValue();
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
    
        InputStream input = new BufferedInputStream(getJobOutputResult.getBody());
        OutputStream output = null;
        try {
            output = new BufferedOutputStream(new FileOutputStream(fileName));

            byte[] buffer = new byte[1024 * 1024];

            int bytesRead = 0;
            do {
                bytesRead = input.read(buffer);
                if (bytesRead <= 0) break;
                output.write(buffer, 0, bytesRead);
            } while (bytesRead > 0);
        } catch (IOException e) {
            throw new AmazonClientException("Unable to save archive", e);
        } finally {
            try {input.close();}  catch (Exception e) {}
            try {output.close();} catch (Exception e) {}
        }
        System.out.println("Retrieved archive to " + fileName);
    }
    
    private static void cleanUp() {
        snsClient.unsubscribe(new UnsubscribeRequest(snsSubscriptionARN));
        snsClient.deleteTopic(new DeleteTopicRequest(snsTopicARN));
        sqsClient.deleteQueue(new DeleteQueueRequest(sqsQueueURL));
    }
}
```

### Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for Java—Download Output in Chunks<a name="downloading-an-archive-with-range-using-java-example"></a>

The following Java code example retrieves an archive from S3 Glacier\. The code example downloads the job output in chunks by specifying byte range in a `GetJobOutputRequest` object\.

```
import java.io.BufferedInputStream;
import java.io.ByteArrayInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import com.fasterxml.jackson.core.JsonFactory;
import com.fasterxml.jackson.core.JsonParseException;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;

import com.amazonaws.auth.policy.Policy;
import com.amazonaws.auth.policy.Principal;
import com.amazonaws.auth.policy.Resource;
import com.amazonaws.auth.policy.Statement;
import com.amazonaws.auth.policy.Statement.Effect;
import com.amazonaws.auth.policy.actions.SQSActions;
import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.TreeHashGenerator;
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


public class ArchiveDownloadLowLevelWithRange {
    
    public static String vaultName = "*** provide vault name ***";
    public static String archiveId = "*** provide archive id ***";
    public static String snsTopicName = "glacier-temp-sns-topic";
    public static String sqsQueueName = "glacier-temp-sqs-queue";
    public static long downloadChunkSize = 4194304; // 4 MB  
    public static String sqsQueueARN;
    public static String sqsQueueURL;
    public static String snsTopicARN;
    public static String snsSubscriptionARN;
    public static String fileName = "*** provide file name to save archive to ***";
    public static String region   = "*** region ***";
    public static long sleepTime  = 600; 
    
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
            
            long archiveSizeInBytes = waitForJobToComplete(jobId, sqsQueueURL);
            if (archiveSizeInBytes==-1) { throw new Exception("Job did not complete successfully."); }
            
            downloadJobOutput(jobId, archiveSizeInBytes);
            
            cleanUp();
            
        } catch (Exception e) {
            System.err.println("Archive retrieval failed.");
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
            .withType("archive-retrieval")
            .withArchiveId(archiveId)
            .withSNSTopic(snsTopicARN);
        
        InitiateJobRequest request = new InitiateJobRequest()
            .withVaultName(vaultName)
            .withJobParameters(jobParameters);
        
        InitiateJobResult response = client.initiateJob(request);
        
        return response.getJobId();
    }
    
    private static long waitForJobToComplete(String jobId, String sqsQueueUrl) throws InterruptedException, JsonParseException, IOException {
        
        Boolean messageFound = false;
        Boolean jobSuccessful = false;
        long archiveSizeInBytes = -1;
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
                    archiveSizeInBytes = jobDescNode.get("ArchiveSizeInBytes").longValue();
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
        return (messageFound && jobSuccessful) ? archiveSizeInBytes : -1;
    }
    
    private static void downloadJobOutput(String jobId, long archiveSizeInBytes) throws IOException {
        
        if (archiveSizeInBytes < 0) {
            System.err.println("Nothing to download.");
            return;
        }

        System.out.println("archiveSizeInBytes: " + archiveSizeInBytes);
        FileOutputStream fstream = new FileOutputStream(fileName);
        long startRange = 0;
        long endRange = (downloadChunkSize > archiveSizeInBytes) ? archiveSizeInBytes -1 : downloadChunkSize - 1;

        do {

            GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
                .withVaultName(vaultName)
                .withRange("bytes=" + startRange + "-" + endRange)
                .withJobId(jobId);
            GetJobOutputResult getJobOutputResult = client.getJobOutput(getJobOutputRequest);

            BufferedInputStream is = new BufferedInputStream(getJobOutputResult.getBody());     
            byte[] buffer = new byte[(int)(endRange - startRange + 1)];

            System.out.println("Checksum received: " + getJobOutputResult.getChecksum());
            System.out.println("Content range " + getJobOutputResult.getContentRange());

            
            int totalRead = 0;
            while (totalRead < buffer.length) {
                int bytesRemaining = buffer.length - totalRead;
                int read = is.read(buffer, totalRead, bytesRemaining);
                if (read > 0) {
                    totalRead = totalRead + read;                             
                } else {
                    break;
                }
                
            }
            System.out.println("Calculated checksum: " + TreeHashGenerator.calculateTreeHash(new ByteArrayInputStream(buffer)));
            System.out.println("read = " + totalRead);
            fstream.write(buffer);
            
            startRange = startRange + (long)totalRead;
            endRange = ((endRange + downloadChunkSize) >  archiveSizeInBytes) ? archiveSizeInBytes : (endRange + downloadChunkSize); 
            is.close();
        } while (endRange <= archiveSizeInBytes  && startRange < archiveSizeInBytes);
        
        fstream.close();
        System.out.println("Retrieved file to " + fileName);

    }
    
    private static void cleanUp() {
        snsClient.unsubscribe(new UnsubscribeRequest(snsSubscriptionARN));
        snsClient.deleteTopic(new DeleteTopicRequest(snsTopicARN));
        sqsClient.deleteQueue(new DeleteQueueRequest(sqsQueueURL));
    }
}
```