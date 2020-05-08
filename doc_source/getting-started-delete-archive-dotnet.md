# Delete an Archive from a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="getting-started-delete-archive-dotnet"></a>

The following C\# code example uses the high\-level API of the AWS SDK for \.NET to delete the archive you uploaded in the previous step\. In the code example, note the following:
+ The example creates an instance of the `ArchiveTransferManager` class for the specified Amazon S3 Glacier \(S3 Glacier\) Region endpoint\.
+ The code example uses the US West \(Oregon\) Region \(`us-west-2`\) to match the location where you created the vault previously in [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\. 
+ The example uses the `Delete` method of the `ArchiveTransferManager` class provided as part of the high\-level API of the AWS SDK for \.NET\. 

For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the archive ID of the file you uploaded in [Step 3: Upload an Archive to a Vault in Amazon S3 Glacier](getting-started-upload-archive.md)\. 

**Example — Deleting an Archive Using the High\-Level API of the AWS SDK for \.NET**  <a name="GS_ExampleDeleteArchiveDotNet"></a>

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveDeleteHighLevel_GettingStarted
  {
    static string vaultName = "examplevault";
    static string archiveId = "*** Provide archive ID ***";

    public static void Main(string[] args)
    {
      try
      {
        var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
        manager.DeleteArchive(vaultName, archiveId);
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