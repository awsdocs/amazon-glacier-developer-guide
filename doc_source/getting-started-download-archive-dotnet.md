# Download an Archive from a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="getting-started-download-archive-dotnet"></a>

The following C\# code example uses the high\-level API of the AWS SDK for \.NET to download the archive you uploaded previously in [Upload an Archive to a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET](getting-started-upload-archive-dotnet.md)\. In the code example, note the following:
+ The example creates an instance of the `ArchiveTransferManager` class for the specified Amazon S3 Glacier \(S3 Glacier\) Region endpoint\.
+ The code example uses the US West \(Oregon\) Region \(`us-west-2`\) to match the location where you created the vault previously in [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\. 
+ The example uses the `Download` method of the `ArchiveTransferManager` class to download your archive\. The example creates an Amazon SNS topic, and an Amazon Simple Queue Service queue that is subscribed to that topic\. If you created an IAM administrative user as instructed in [Step 1: Before You Begin with Amazon S3 Glacier](getting-started-before-you-begin.md) your user has the necessary IAM permissions for the creation and use of the Amazon SNS topic and Amazon SQS queue\.
+ The example then initiates the archive retrieval job and polls the queue for the archive to be available\. When the archive is available, download begins\. For information about retrieval times, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)

For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the archive ID of the file you uploaded in [Step 3: Upload an Archive to a Vault in Amazon S3 Glacier](getting-started-upload-archive.md)\. 

**Example — Download an Archive Using the High\-Level API of the AWS SDK for \.NET**  <a name="GS_ExampleDownloadArchiveDotNet"></a>

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
    class ArchiveDownloadHighLevel_GettingStarted
    {
        static string vaultName = "examplevault";
        static string archiveId = "*** Provide archive ID ***";
        static string downloadFilePath = "*** Provide the file name and path to where to store the download ***";

        public static void Main(string[] args)
        {
            try
            {
                var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);

                var options = new DownloadOptions();
                options.StreamTransferProgress += ArchiveDownloadHighLevel_GettingStarted.progress;
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