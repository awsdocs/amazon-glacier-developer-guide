# Download an Amazon S3 Glacier archive using an AWS SDK<a name="example_glacier_DownloadArchive_section"></a>

The following code example shows how to download an Amazon S3 Glacier archive\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Glacier#code-examples)\. 
  

```
    using System;
    using Amazon;
    using Amazon.Glacier;
    using Amazon.Glacier.Transfer;
    using Amazon.Runtime;

    class DownloadArchiveHighLevel
    {
        private static readonly string VaultName = "examplevault";
        private static readonly string ArchiveId = "*** Provide archive ID ***";
        private static readonly string DownloadFilePath = "*** Provide the file name and path to where to store the download ***";
        private static int currentPercentage = -1;

        static void Main()
        {
            try
            {
                var manager = new ArchiveTransferManager(RegionEndpoint.USEast2);

                var options = new DownloadOptions
                {
                    StreamTransferProgress = Progress,
                };

                // Download an archive.
                Console.WriteLine("Intiating the archive retrieval job and then polling SQS queue for the archive to be available.");
                Console.WriteLine("Once the archive is available, downloading will begin.");
                manager.DownloadAsync(VaultName, ArchiveId, DownloadFilePath, options);
                Console.WriteLine("To continue, press Enter");
                Console.ReadKey();
            }
            catch (AmazonGlacierException ex)
            {
                Console.WriteLine(ex.Message);
            }

            Console.WriteLine("To continue, press Enter");
            Console.ReadKey();
        }

        static void Progress(object sender, StreamTransferProgressArgs args)
        {
            if (args.PercentDone != currentPercentage)
            {
                currentPercentage = args.PercentDone;
                Console.WriteLine("Downloaded {0}%", args.PercentDone);
            }
        }
    }
```

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.