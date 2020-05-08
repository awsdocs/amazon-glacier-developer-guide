# Upload an Archive to a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="getting-started-upload-archive-dotnet"></a>

The following C\# code example uses the high\-level API of the AWS SDK for \.NET to upload a sample archive to the vault\. In the code example, note the following:
+ The example creates an instance of the `ArchiveTransferManager` class for the specified Amazon S3 Glacier \(S3 Glacier\) Region endpoint\.
+ The code example uses the US West \(Oregon\) Region \(`us-west-2`\) to match the location where you created the vault previously in [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\. 
+ The example uses the `Upload` method of the `ArchiveTransferManager` class to upload your archive\. For small archives, this method uploads the archive directly to S3 Glacier\. For larger archives, this method uses the multipart upload API in S3 Glacier to split the upload into multiple parts for better error recovery, if any errors are encountered while streaming the data to S3 Glacier\.

For step\-by\-step instructions on how to run the following example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the name of your vault and the name of the archive file to upload\.  

**Note**  
S3 Glacier keeps an inventory of all the archives in your vaults\. When you upload the archive in the following example, it will not appear in a vault in the management console until the vault inventory has been updated\. This update usually happens once a day\. 

**Example — Uploading an Archive Using the High\-Level API of the AWS SDK for \.NET**  <a name="GS_ExampleUploadArchiveDotNet"></a>

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
    class ArchiveUploadHighLevel_GettingStarted
    {
        static string vaultName = "examplevault";
        static string archiveToUpload = "*** Provide file name (with full path) to upload ***";

        public static void Main(string[] args)
        {
            try
            {
                var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
                // Upload an archive.
                string archiveId = manager.Upload(vaultName, "getting started archive test", archiveToUpload).ArchiveId;
                Console.WriteLine("Copy and save the following Archive ID for the next step."); 
                Console.WriteLine("Archive ID: {0}", archiveId);
                Console.WriteLine("To continue, press Enter");
                Console.ReadKey();
            }
            catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
            catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
            catch (Exception e) { Console.WriteLine(e.Message); }
            Console.WriteLine("To continue, press Enter");
            Console.ReadKey();
        }
    }
}
```