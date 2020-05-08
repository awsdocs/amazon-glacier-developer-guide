# Downloading a Vault Inventory in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="retrieving-vault-inventory-sdk-dotnet"></a>

The following are the steps to retrieve a vault inventory using the low\-level API of the AWS SDK for \.NET\. The high\-level API does not support retrieving a vault inventory\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the vault resides\. All operations you perform using this client apply to that AWS Region\.

1. Initiate an inventory retrieval job by executing the `InitiateJob` method\.

   You provide job information in an `InitiateJobRequest` object\. Amazon S3 Glacier \(S3 Glacier\) returns a job ID in response\. The response is available in an instance of the `InitiateJobResponse` class\.

   ```
   AmazonGlacierClient client;
   client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2);
   
   InitiateJobRequest initJobRequest = new InitiateJobRequest()
   {
     VaultName = vaultName,
     JobParameters = new JobParameters()
     {
       Type = "inventory-retrieval",
       SNSTopic = "*** Provide Amazon SNS topic arn ***",
     }
   };
   InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
   string jobId = initJobResponse.JobId;
   ```

1. Wait for the job to complete\.

   You must wait until the job output is ready for you to download\. If you have either set a notification configuration on the vault identifying an Amazon Simple Notification Service \(Amazon SNS\) topic, or specified an Amazon SNS topic when you initiated a job, S3 Glacier sends a message to that topic after it completes the job\. The code example given in the following section uses Amazon SNS for S3 Glacier to publish a message\.

   You can also poll S3 Glacier by calling the `DescribeJob` method to determine job completion status\. Although using Amazon SNS topic for notification is the recommended approach\. 

1. Download the job output \(vault inventory data\) by executing the `GetJobOutput` method\.

   You provide your account ID, vault name, and the job ID information by creating an instance of the `GetJobOutputRequest` class\. If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\. 

   The output that S3 Glacier returns is available in the `GetJobOutputResponse` object\. 

   ```
   GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
   {
     JobId = jobId,
     VaultName = vaultName
   };
   						       
   GetJobOutputResponse getJobOutputResponse = client.GetJobOutput(getJobOutputRequest); 
   using (Stream webStream = getJobOutputResponse.Body)
   {
      using (Stream fileToSave = File.OpenWrite(fileName))
      {
        CopyStream(webStream, fileToSave);
      }
   }
   ```
**Note**  
For information about the job related underlying REST API, see [Job Operations](job-operations.md)\.

## Example: Retrieving a Vault Inventory Using the Low\-Level API of the AWS SDK for \.NET<a name="creating-vaults-sdk-dotnet-example-inventory"></a>

The following C\# code example retrieves the vault inventory for the specified vault\. 

The example performs the following tasks:
+ Set up an Amazon SNS topic\.

  S3 Glacier sends notification to this topic after it completes the job\. 
+ Set up an Amazon SQS queue\. 

  The example attaches a policy to the queue to enable the Amazon SNS topic to post messages\. 
+ Initiate a job to download the specified archive\.

  In the job request, the example specifies the Amazon SNS topic so that S3 Glacier can send a message after it completes the job\.
+ Periodically check the Amazon SQS queue for a message\. 

  If there is a message, parse the JSON and check if the job completed successfully\. If it did, download the archive\. The code example uses the JSON\.NET library \(see [JSON\.NET](http://json.codeplex.com/)\) to parse the JSON\.
+ Clean up by deleting the Amazon SNS topic and the Amazon SQS queue it created\.

**Example**  

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;
using Amazon.SimpleNotificationService;
using Amazon.SimpleNotificationService.Model;
using Amazon.SQS;
using Amazon.SQS.Model;
using Newtonsoft.Json;

namespace glacier.amazon.com.docsamples
{
  class VaultInventoryJobLowLevelUsingSNSSQS
  {
    static string topicArn;
    static string queueUrl;
    static string queueArn;
    static string vaultName = "*** Provide vault name ***";
    static string fileName  = "*** Provide file name and path where to store inventory ***";
    static AmazonSimpleNotificationServiceClient snsClient;
    static AmazonSQSClient sqsClient;
    const string SQS_POLICY =
        "{" +
        "    \"Version\" : \"2012-10-17\"," +
        "    \"Statement\" : [" +
        "        {" +
        "            \"Sid\" : \"sns-rule\"," +
        "            \"Effect\" : \"Allow\"," +
        "            \"Principal\" : {\"AWS\" : \"arn:aws:iam::123456789012:root\" }," +
        "            \"Action\"    : \"sqs:SendMessage\"," +
        "            \"Resource\"  : \"{QuernArn}\"," +
        "            \"Condition\" : {" +
        "                \"ArnLike\" : {" +
        "                    \"aws:SourceArn\" : \"{TopicArn}\"" +
        "                }" +
        "            }" +
        "        }" +
        "    ]" +
        "}";

    public static void Main(string[] args)
    {
      AmazonGlacierClient client;
      try
      {
        using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2))
        {
            Console.WriteLine("Setup SNS topic and SQS queue."); 
            SetupTopicAndQueue();
            Console.WriteLine("To continue, press Enter"); Console.ReadKey();
            
            Console.WriteLine("Retrieve Inventory List");
            GetVaultInventory(client);
        }
        Console.WriteLine("Operations successful.");
        Console.WriteLine("To continue, press Enter"); Console.ReadKey();
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      finally
      {        
       // Delete SNS topic and SQS queue.
       snsClient.DeleteTopic(new DeleteTopicRequest() { TopicArn = topicArn });
       sqsClient.DeleteQueue(new DeleteQueueRequest() { QueueUrl = queueUrl });
      }
    }

    static void SetupTopicAndQueue()
    {
      long ticks = DateTime.Now.Ticks;
      
      // Setup SNS topic.
      snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
      sqsClient = new AmazonSQSClient(Amazon.RegionEndpoint.USWest2);

      topicArn = snsClient.CreateTopic(new CreateTopicRequest { Name = "GlacierDownload-" + ticks }).TopicArn;
      Console.Write("topicArn: "); Console.WriteLine(topicArn);
 
      CreateQueueRequest createQueueRequest =  new CreateQueueRequest();
      createQueueRequest.QueueName = "GlacierDownload-" + ticks;
      CreateQueueResponse createQueueResponse = sqsClient.CreateQueue(createQueueRequest);
      queueUrl = createQueueResponse.QueueUrl;
      Console.Write("QueueURL: "); Console.WriteLine(queueUrl);

      GetQueueAttributesRequest getQueueAttributesRequest = new GetQueueAttributesRequest();
      getQueueAttributesRequest.AttributeNames = new List<string> { "QueueArn" };
      getQueueAttributesRequest.QueueUrl = queueUrl;
      GetQueueAttributesResponse response = sqsClient.GetQueueAttributes(getQueueAttributesRequest);
      queueArn = response.QueueARN;
      Console.Write("QueueArn: ");Console.WriteLine(queueArn);

      // Setup the Amazon SNS topic to publish to the SQS queue.
      snsClient.Subscribe(new SubscribeRequest()
      {
        Protocol = "sqs",
        Endpoint = queueArn,
        TopicArn = topicArn
      });

      // Add the policy to the queue so SNS can send messages to the queue.
      var policy = SQS_POLICY.Replace("{TopicArn}", topicArn).Replace("{QuernArn}", queueArn);
     
      sqsClient.SetQueueAttributes(new SetQueueAttributesRequest()
      {
          QueueUrl = queueUrl,
          Attributes = new Dictionary<string, string>
          {
              { QueueAttributeName.Policy, policy }
          }
      });

    }
 
    static void GetVaultInventory(AmazonGlacierClient client)
    {
      // Initiate job.
      InitiateJobRequest initJobRequest = new InitiateJobRequest()
      {
        VaultName = vaultName,
        JobParameters = new JobParameters()
        {
          Type = "inventory-retrieval",
          Description = "This job is to download a vault inventory.",
          SNSTopic = topicArn,
        }
      };
   
      InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
      string jobId = initJobResponse.JobId;

      // Check queue for a message and if job completed successfully, download inventory.
      ProcessQueue(jobId, client);    
    }

    private static void ProcessQueue(string jobId, AmazonGlacierClient client)
    {
      ReceiveMessageRequest receiveMessageRequest = new ReceiveMessageRequest() { QueueUrl = queueUrl, MaxNumberOfMessages = 1 };
      bool jobDone = false;
      while (!jobDone)
      {
        Console.WriteLine("Poll SQS queue"); 
        ReceiveMessageResponse receiveMessageResponse = sqsClient.ReceiveMessage(receiveMessageRequest);
        if (receiveMessageResponse.Messages.Count == 0)
        {
          Thread.Sleep(10000 * 60);
          continue;
        }
        Console.WriteLine("Got message");
        Message message = receiveMessageResponse.Messages[0];
        Dictionary<string, string> outerLayer = JsonConvert.DeserializeObject<Dictionary<string, string>>(message.Body);
        Dictionary<string, object> fields = JsonConvert.DeserializeObject<Dictionary<string, object>>(outerLayer["Message"]);
        string statusCode = fields["StatusCode"] as string;

        if (string.Equals(statusCode, GlacierUtils.JOB_STATUS_SUCCEEDED, StringComparison.InvariantCultureIgnoreCase))
        {
          Console.WriteLine("Downloading job output");
          DownloadOutput(jobId, client); // Save job output to the specified file location.
        }
        else if (string.Equals(statusCode, GlacierUtils.JOB_STATUS_FAILED, StringComparison.InvariantCultureIgnoreCase))
          Console.WriteLine("Job failed... cannot download the inventory.");

        jobDone = true;
        sqsClient.DeleteMessage(new DeleteMessageRequest() { QueueUrl = queueUrl, ReceiptHandle = message.ReceiptHandle });
      }
    }

    private static void DownloadOutput(string jobId, AmazonGlacierClient client)
    {
      GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
      {      
        JobId = jobId,
        VaultName = vaultName
      };
      
      GetJobOutputResponse getJobOutputResponse = client.GetJobOutput(getJobOutputRequest); 
      using (Stream webStream = getJobOutputResponse.Body)
      {
        using (Stream fileToSave = File.OpenWrite(fileName))
        {
          CopyStream(webStream, fileToSave);
        }
      }
    }

    public static void CopyStream(Stream input, Stream output)
    {
      byte[] buffer = new byte[65536];
      int length;
      while ((length = input.Read(buffer, 0, buffer.Length)) > 0)
      {
        output.Write(buffer, 0, length);
      }
    }
  }
}
```