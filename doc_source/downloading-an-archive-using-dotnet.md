# Downloading an Archive in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="downloading-an-archive-using-dotnet"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to download an archive\.

**Topics**
+ [Downloading an Archive Using the High\-Level API of the AWS SDK for \.NET](#downloading-an-archive-using-dotnet-highlevel-api)
+ [Downloading an Archive Using the Low\-Level API of the AWS SDK for \.NET](#downloading-an-archive-using-dotnet-lowlevel-api)

## Downloading an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="downloading-an-archive-using-dotnet-highlevel-api"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `Download` method you can use to download an archive\. 

**Important**  
The `ArchiveTransferManager` class creates an Amazon Simple Notification Service \(Amazon SNS\) topic, and an Amazon Simple Queue Service \(Amazon SQS\) queue that is subscribed to that topic\. It then initiates the archive retrieval job and polls the queue for the archive to be available\. When the archive is available, download begins\. For information about retrieval times, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)

### Example: Downloading an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="download-archives-dotnet-highlevel-example"></a>

The following C\# code example downloads an archive from a vault \(`examplevault`\) in the US West \(Oregon\) Region\. 

For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with an existing archive ID and the local file path where you want to save the downloaded archive\.

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveDownloadHighLevel
  {
    static string vaultName        = "examplevault";
    static string archiveId        = "*** Provide archive ID ***";
    static string downloadFilePath = "*** Provide the file name and path to where to store the download ***";

    public static void Main(string[] args)
    {
      try
      {
        var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);

        var options = new DownloadOptions();
        options.StreamTransferProgress += ArchiveDownloadHighLevel.progress;
        // Download an archive.
        Console.WriteLine("Intiating the archive retrieval job and then polling SQS queue for the archive to be available.");
        Console.WriteLine("Once the archive is available, downloading will begin.");
        manager.Download(vaultName, archiveId, downloadFilePath, options);
        Console.WriteLine("To continue, press Enter");
        Console.ReadKey();
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      Console.WriteLine("To continue, press Enter");
      Console.ReadKey();
    }

    static int currentPercentage = -1;
    static void progress(object sender, StreamTransferProgressArgs args)
    {
      if (args.PercentDone != currentPercentage)
      {
        currentPercentage = args.PercentDone;
        Console.WriteLine("Downloaded {0}%", args.PercentDone);
      }
    }
  }
}
```

## Downloading an Archive Using the Low\-Level API of the AWS SDK for \.NET<a name="downloading-an-archive-using-dotnet-lowlevel-api"></a>

The following are the steps for downloading an Amazon S3 Glacier \(S3 Glacier\) archive using the low\-level API of the AWS SDK for \.NET\. 

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region from where you want to download the archive\. All operations you perform using this client apply to that AWS Region\.

1. Initiate an `archive-retrieval` job by executing the `InitiateJob` method\.

   You provide job information, such as the archive ID of the archive you want to download and the optional Amazon SNS topic to which you want S3 Glacier to post a job completion message, by creating an instance of the `InitiateJobRequest` class\. S3 Glacier returns a job ID in response\. The response is available in an instance of the `InitiateJobResponse` class\.

   ```
   AmazonGlacierClient client;
   client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2);
   
   InitiateJobRequest initJobRequest = new InitiateJobRequest()
   {
     VaultName = vaultName,
     JobParameters = new JobParameters()
     {
       Type      = "archive-retrieval",
       ArchiveId = "*** Provide archive id ***",
       SNSTopic  = "*** Provide Amazon SNS topic ARN ***",
     }
   };
   
   InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
   string jobId = initJobResponse.JobId;
   ```

   You can optionally specify a byte range to request S3 Glacier to prepare only a portion of the archive as shown in the following request\. The request specifies S3 Glacier to prepare only the 1 MB to 2 MB portion of the archive\.

   ```
   AmazonGlacierClient client;
   client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2);
   
   
   InitiateJobRequest initJobRequest = new InitiateJobRequest()
   {
     VaultName = vaultName,
     JobParameters = new JobParameters()
     {
       Type      = "archive-retrieval",
       ArchiveId = "*** Provide archive id ***",
       SNSTopic  = "*** Provide Amazon SNS topic ARN ***",
     }
   };
   // Specify byte range.
   int ONE_MEG = 1048576;
   initJobRequest.JobParameters.RetrievalByteRange = string.Format("{0}-{1}", ONE_MEG, 2 * ONE_MEG -1);
   
   InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
   string jobId = initJobResponse.JobId;
   ```

1. Wait for the job to complete\.

   You must wait until the job output is ready for you to download\. If you have either set a notification configuration on the vault identifying an Amazon Simple Notification Service \(Amazon SNS\) topic or specified an Amazon SNS topic when you initiated a job, S3 Glacier sends a message to that topic after it completes the job\. The code example given in the following section uses Amazon SNS for S3 Glacier to publish a message\.

   You can also poll S3 Glacier by calling the `DescribeJob` method to determine the job completion status\. Although, using an Amazon SNS topic for notification is the recommended approach \. 

1. Download the job output \(archive data\) by executing the `GetJobOutput` method\.

   You provide the request information such as the job ID and vault name by creating an instance of the `GetJobOutputRequest` class\. The output that S3 Glacier returns is available in the `GetJobOutputResponse` object\. 

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

   The preceding code snippet downloads the entire job output\. You can optionally retrieve only a portion of the output, or download the entire output in smaller chunks by specifying the byte range in your `GetJobOutputRequest`\. 

   ```
   GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
   {
     JobId = jobId,
     VaultName = vaultName
   };
   getJobOutputRequest.SetRange(0, 1048575); // Download only the first 1 MB chunk of the output.
   ```

   In response to your `GetJobOutput` call, S3 Glacier returns the checksum of the portion of the data you downloaded, if certain conditions are met\. For more information, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\.

   To verify there are no errors in the download, you can then compute the checksum on the client\-side and compare it with the checksum S3 Glacier sent in the response\. 

   For an archive retrieval job with the optional range specified, when you get the job description, it includes the checksum of the range you are retrieving \(SHA256TreeHash\)\.You can use this value to further verify the accuracy of the entire byte range that you later download\. For example, if you initiate a job to retrieve a tree\-hash aligned archive range and then download output in chunks such that each of your `GetJobOutput` requests return a checksum, then you can compute checksum of each portion you download on the client\-side and then compute the tree hash\. You can compare it with the checksum S3 Glacier returns in response to your describe job request to verify that the entire byte range you have downloaded is the same as the byte range that is stored in S3 Glacier\. 

   For a working example, see [Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for \.NET—Download Output in Chunks](#creating-vaults-sdk-dotnet-example2)\.

### Example 1: Retrieving an Archive Using the Low\-Level API of the AWS SDK for \.NET<a name="creating-vaults-sdk-dotnet-example-retrieve"></a>

The following C\# code example downloads an archive from the specified vault\. After the job completes, the example downloads the entire output in a single `GetJobOutput` call\. For an example of downloading output in chunks, see [Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for \.NET—Download Output in Chunks](#creating-vaults-sdk-dotnet-example2)\.

The example performs the following tasks:
+ Sets up an Amazon Simple Notification Service \(Amazon SNS\) topic 

  S3 Glacier sends a notification to this topic after it completes the job\. 
+ Sets up an Amazon Simple Queue Service \(Amazon SQS\) queue\. 

  The example attaches a policy to the queue to enable the Amazon SNS topic to post messages\. 
+ Initiates a job to download the specified archive\.

  In the job request, the example specifies the Amazon SNS topic so that S3 Glacier can send a message after it completes the job\.
+ Periodically checks the Amazon SQS queue for a message\. 

  If there is a message, parse the JSON and check if the job completed successfully\. If it did, download the archive\. The code example uses the JSON\.NET library \(see [JSON\.NET](http://json.codeplex.com/)\) to parse the JSON\.
+ Cleans up by deleting the Amazon SNS topic and the Amazon SQS queue it created\.

```
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;
using Amazon.SimpleNotificationService;
using Amazon.SimpleNotificationService.Model;
using Amazon.SQS;
using Amazon.SQS.Model;
using Newtonsoft.Json;

namespace glacier.amazon.com.docsamples
{
  class ArchiveDownloadLowLevelUsingSNSSQS
  {
    static string topicArn;
    static string queueUrl;
    static string queueArn;
    static string vaultName = "*** Provide vault name ***";
    static string archiveID = "*** Provide archive ID ***";
    static string fileName  = "*** Provide the file name and path to where to store downloaded archive ***";
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
          Console.WriteLine("Retrieving...");
          RetrieveArchive(client);
        }
        Console.WriteLine("Operations successful. To continue, press Enter");
        Console.ReadKey();
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
      snsClient = new AmazonSimpleNotificationServiceClient(Amazon.RegionEndpoint.USWest2);
      sqsClient = new AmazonSQSClient(Amazon.RegionEndpoint.USWest2);

      long ticks = DateTime.Now.Ticks;
      topicArn = snsClient.CreateTopic(new CreateTopicRequest { Name = "GlacierDownload-" + ticks }).TopicArn;
      Console.Write("topicArn: "); Console.WriteLine(topicArn);

      CreateQueueRequest createQueueRequest = new CreateQueueRequest();
      createQueueRequest.QueueName = "GlacierDownload-" + ticks;
      CreateQueueResponse createQueueResponse = sqsClient.CreateQueue(createQueueRequest);
      queueUrl = createQueueResponse.QueueUrl;
      Console.Write("QueueURL: "); Console.WriteLine(queueUrl);

      GetQueueAttributesRequest getQueueAttributesRequest = new GetQueueAttributesRequest();
      getQueueAttributesRequest.AttributeNames = new List<string> { "QueueArn" };
      getQueueAttributesRequest.QueueUrl = queueUrl;
      GetQueueAttributesResponse response = sqsClient.GetQueueAttributes(getQueueAttributesRequest);
      queueArn = response.QueueARN;
      Console.Write("QueueArn: "); Console.WriteLine(queueArn);

      // Setup the Amazon SNS topic to publish to the SQS queue.
      snsClient.Subscribe(new SubscribeRequest()
      {
        Protocol = "sqs",
        Endpoint = queueArn,
        TopicArn = topicArn
      });

      // Add policy to the queue so SNS can send messages to the queue.
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

    static void RetrieveArchive(AmazonGlacierClient client)
    {
      // Initiate job.
      InitiateJobRequest initJobRequest = new InitiateJobRequest()
      {
        VaultName = vaultName,
        JobParameters = new JobParameters()
        {
          Type = "archive-retrieval", 
          ArchiveId = archiveID,
          Description = "This job is to download archive.",
          SNSTopic = topicArn,
        }
      };
      InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
      string jobId = initJobResponse.JobId;

      // Check queue for a message and if job completed successfully, download archive.
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
          Console.WriteLine("Job failed... cannot download the archive.");

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

### Example 2: Retrieving an Archive Using the Low\-Level API of the AWS SDK for \.NET—Download Output in Chunks<a name="creating-vaults-sdk-dotnet-example2"></a>

The following C\# code example retrieves an archive from S3 Glacier\. The code example downloads the job output in chunks by specifying the byte range in a `GetJobOutputRequest` object\.

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
using System.Collections.Specialized;

namespace glacier.amazon.com.docsamples
{
  class ArchiveDownloadLowLevelUsingSQLSNSOutputUsingRange
  {
    static string topicArn;
    static string queueUrl;
    static string queueArn;
    static string vaultName = "*** Provide vault name ***";
    static string archiveId = "*** Provide archive ID ***";
    static string fileName  = "*** Provide the file name and path to where to store downloaded archive ***";
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
        "            \"Action\"  : \"sqs:SendMessage\"," +
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

              Console.WriteLine("Download archive");
              DownloadAnArchive(archiveId, client);
        }
        Console.WriteLine("Operations successful. To continue, press Enter");
        Console.ReadKey();
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

      CreateQueueRequest createQueueRequest = new CreateQueueRequest();
      createQueueRequest.QueueName = "GlacierDownload-" + ticks;
      CreateQueueResponse createQueueResponse = sqsClient.CreateQueue(createQueueRequest);
      queueUrl = createQueueResponse.QueueUrl;
      Console.Write("QueueURL: "); Console.WriteLine(queueUrl);

      GetQueueAttributesRequest getQueueAttributesRequest = new GetQueueAttributesRequest();
      getQueueAttributesRequest.AttributeNames = new List<string> { "QueueArn" };
      getQueueAttributesRequest.QueueUrl = queueUrl;
      GetQueueAttributesResponse response = sqsClient.GetQueueAttributes(getQueueAttributesRequest);
      queueArn = response.QueueARN;
      Console.Write("QueueArn: "); Console.WriteLine(queueArn);

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

    static void DownloadAnArchive(string archiveId, AmazonGlacierClient client)
    {
      // Initiate job.
      InitiateJobRequest initJobRequest = new InitiateJobRequest()
      {

        VaultName = vaultName,
        JobParameters = new JobParameters()
        {
          Type = "archive-retrieval",
          ArchiveId = archiveId,
          Description = "This job is to download the archive.",
          SNSTopic = topicArn,
        }
      };
      InitiateJobResponse initJobResponse = client.InitiateJob(initJobRequest);
      string jobId = initJobResponse.JobId;

      // Check queue for a message and if job completed successfully, download archive.
      ProcessQueue(jobId, client);
    }

    private static void ProcessQueue(string jobId, AmazonGlacierClient client)
    {
        var receiveMessageRequest = new ReceiveMessageRequest() { QueueUrl = queueUrl, MaxNumberOfMessages = 1 };
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
                long archiveSize = Convert.ToInt64(fields["ArchiveSizeInBytes"]);
                Console.WriteLine("Downloading job output");
                DownloadOutput(jobId, archiveSize, client); // This where we save job output to the specified file location.
            }
            else if (string.Equals(statusCode, GlacierUtils.JOB_STATUS_FAILED, StringComparison.InvariantCultureIgnoreCase))
                Console.WriteLine("Job failed... cannot download the archive.");
            jobDone = true;
            sqsClient.DeleteMessage(new DeleteMessageRequest() { QueueUrl = queueUrl, ReceiptHandle = message.ReceiptHandle });
        }
    }               

    private static void DownloadOutput(string jobId, long archiveSize, AmazonGlacierClient client)
    {
      long partSize = 4 * (long)Math.Pow(2, 20);  // 4 MB.
      using (Stream fileToSave = new FileStream(fileName, FileMode.Create, FileAccess.Write))
      {

        long currentPosition = 0;
        do
        {
          GetJobOutputRequest getJobOutputRequest = new GetJobOutputRequest()
          {
            JobId = jobId,
            VaultName = vaultName
          };

          long endPosition = currentPosition + partSize - 1;
          if (endPosition > archiveSize)
            endPosition = archiveSize;

          getJobOutputRequest.SetRange(currentPosition, endPosition);
          GetJobOutputResponse getJobOutputResponse = client.GetJobOutput(getJobOutputRequest);

          using (Stream webStream = getJobOutputResponse.Body)
          {
            CopyStream(webStream, fileToSave);
          }
          currentPosition += partSize;
        } while (currentPosition < archiveSize);
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