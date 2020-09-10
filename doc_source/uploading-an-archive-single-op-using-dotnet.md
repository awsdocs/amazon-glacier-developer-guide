# Uploading an Archive in a Single Operation Using the AWS SDK for \.NET in Amazon S3 Glacier<a name="uploading-an-archive-single-op-using-dotnet"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to upload an archive in a single operation\.

**Topics**
+ [Uploading an Archive Using the High\-Level API of the AWS SDK for \.NET](#uploading-an-archive-single-op-highlevel-using-dotnet)
+ [Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for \.NET](#uploading-an-archive-single-op-lowlevel-using-dotnet)

## Uploading an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="uploading-an-archive-single-op-highlevel-using-dotnet"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `Upload` method that you can use to upload an archive to a vault\. 

**Note**  
You can use the `Upload` method to upload small or large files\. Depending on the file size you are uploading, this method determines whether to upload it in a single operation or use the multipart upload API to upload the file in parts\.

### Example: Uploading an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="upload-archive-highlevel-any-size-dotnet"></a>

The following C\# code example uploads an archive to a vault \(`examplevault`\) in the US West \(Oregon\) Region\. 

For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the name of the file you want to upload\.

**Example**  

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveUploadHighLevel
  {
    static string vaultName = "examplevault"; 
    static string archiveToUpload = "*** Provide file name (with full path) to upload ***";

    public static void Main(string[] args)
    {
       try
      {
         var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
         // Upload an archive.
         string archiveId = manager.Upload(vaultName, "upload archive test", archiveToUpload).ArchiveId;
         Console.WriteLine("Archive ID: (Copy and save this ID for use in other examples.) : {0}", archiveId);
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

## Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for \.NET<a name="uploading-an-archive-single-op-lowlevel-using-dotnet"></a>

The low\-level API provides methods for all the archive operations\. The following are the steps to upload an archive using the AWS SDK for \.NET\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where you want to upload the archive\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `UploadArchiveRequest` class\.

   In addition to the data you want to upload, You need to provide a checksum \(SHA\-256 tree hash\) of the payload, the vault name, and your account ID\. 

   If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\. 

1. Run the `UploadArchive` method by providing the request object as a parameter\. 

   In response, S3 Glacier returns an archive ID of the newly uploaded archive\. 

### Example: Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for \.NET<a name="upload-archive-single-op-lowlevel-dotnet"></a>

The following C\# code example illustrates the preceding steps\. The example uses the AWS SDK for \.NET to upload an archive to a vault \(`examplevault`\)\. 

**Note**  
For information about the underlying REST API to upload an archive in a single request, see [Upload Archive \(POST archive\)](api-archive-post.md)\.

For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the name of the file you want to upload\.

**Example**  

```
using System;
using System.IO;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveUploadSingleOpLowLevel
  {
    static string vaultName       = "examplevault";
    static string archiveToUpload = "*** Provide file name (with full path) to upload ***";

    public static void Main(string[] args)
    {
      AmazonGlacierClient client;
      try
      {
         using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2))
        {
          Console.WriteLine("Uploading an archive.");
          string archiveId = UploadAnArchive(client);
          Console.WriteLine("Archive ID: {0}", archiveId);
        }
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      Console.WriteLine("To continue, press Enter");
      Console.ReadKey();
    }

    static string UploadAnArchive(AmazonGlacierClient client)
    {
      using (FileStream fileStream = new FileStream(archiveToUpload, FileMode.Open, FileAccess.Read))
      {
        string treeHash = TreeHashGenerator.CalculateTreeHash(fileStream);
        UploadArchiveRequest request = new UploadArchiveRequest()
        {
          VaultName = vaultName,
          Body = fileStream,
          Checksum = treeHash
        };
        UploadArchiveResponse response = client.UploadArchive(request);
        string archiveID = response.ArchiveId;
        return archiveID;
      }
    }
  }
}
```