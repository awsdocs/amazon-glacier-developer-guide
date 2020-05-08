# Uploading Large Archives Using the AWS SDK for \.NET<a name="uploading-an-archive-mpu-using-dotnet"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to upload large archives in parts \(see [Uploading an Archive in Amazon S3 Glacier](uploading-an-archive.md)\)\. 
+ The high\-level API provides a method that you can use to upload archives of any size\. Depending on the file you are uploading, the method either uploads archive in a single operation or uses the multipart upload support in Amazon S3 Glacier \(S3 Glacier\) to upload the archive in parts\.
+ The low\-level API maps close to the underlying REST implementation\. Accordingly, it provides a method to upload smaller archives in one operation and a group of methods that support multipart upload for larger archives\. This section explains uploading large archives in parts using the low\-level API\.

For more information about the high\-level and low\-level APIs, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\.

**Topics**
+ [Uploading Large Archives in Parts Using the High\-Level API of the AWS SDK for \.NET](#uploading-an-archive-in-parts-highlevel-using-dotnet)
+ [Uploading Large Archives in Parts Using the Low\-Level API of the AWS SDK for \.NET](#uploading-an-archive-in-parts-lowlevel-using-dotnet)

## Uploading Large Archives in Parts Using the High\-Level API of the AWS SDK for \.NET<a name="uploading-an-archive-in-parts-highlevel-using-dotnet"></a>

You use the same methods of the high\-level API to upload small or large archives\. Based on the archive size, the high\-level API methods decide whether to upload the archive in a single operation or use the multipart upload API provided by S3 Glacier\. For more information, see [Uploading an Archive Using the High\-Level API of the AWS SDK for \.NET ](uploading-an-archive-single-op-using-dotnet.md#uploading-an-archive-single-op-highlevel-using-dotnet)\.

## Uploading Large Archives in Parts Using the Low\-Level API of the AWS SDK for \.NET<a name="uploading-an-archive-in-parts-lowlevel-using-dotnet"></a>

For granular control of the upload, you can use the low\-level API, where you can configure the request and process the response\. The following are the steps to upload large archives in parts using the AWS SDK for \.NET\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where you want to save the archive\. All operations you perform using this client apply to that AWS Region\. 

1. Initiate multipart upload by calling the `InitiateMultipartUpload` method\.

   You need to provide the vault name to which you want to upload the archive, the part size you want to use to upload archive parts, and an optional description\. You provide this information by creating an instance of the `InitiateMultipartUploadRequest` class\. In response, S3 Glacier returns an upload ID\.

1. Upload parts by calling the `UploadMultipartPart` method\. 

   For each part you upload, You need to provide the vault name, the byte range in the final assembled archive that will be uploaded in this part, the checksum of the part data, and the upload ID\. 

1. Complete the multipart upload by calling the `CompleteMultipartUpload` method\.

   You need to provide the upload ID, the checksum of the entire archive, the archive size \(combined size of all parts you uploaded\), and the vault name\. S3 Glacier constructs the archive from the uploaded parts and returns an archive ID\.

### Example: Uploading a Large Archive in Parts Using the AWS SDK for \.NET<a name="upload-archive-mpu-dotnet-example"></a>

The following C\# code example uses the AWS SDK for \.NET to upload an archive to a vault \(`examplevault`\)\. For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the name of a file you want to upload\.

**Example**  

```
using System;
using System.Collections.Generic;
using System.IO;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveUploadMPU
  {
    static string vaultName       = "examplevault";
    static string archiveToUpload = "*** Provide file name (with full path) to upload ***";
    static long partSize          = 4194304; // 4 MB.

    public static void Main(string[] args)
    {
      AmazonGlacierClient client;
      List<string> partChecksumList = new List<string>();
      try
      {
         using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2)) 
        {
          Console.WriteLine("Uploading an archive.");
          string uploadId  = InitiateMultipartUpload(client);
          partChecksumList = UploadParts(uploadId, client);
          string archiveId = CompleteMPU(uploadId, client, partChecksumList);
          Console.WriteLine("Archive ID: {0}", archiveId);
        }
        Console.WriteLine("Operations successful. To continue, press Enter");
        Console.ReadKey();
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      Console.WriteLine("To continue, press Enter");
      Console.ReadKey();
    }

    static string InitiateMultipartUpload(AmazonGlacierClient client)
    {
      InitiateMultipartUploadRequest initiateMPUrequest = new InitiateMultipartUploadRequest()
      {

        VaultName = vaultName,
        PartSize = partSize,
        ArchiveDescription = "Test doc uploaded using MPU."
      };

      InitiateMultipartUploadResponse initiateMPUresponse = client.InitiateMultipartUpload(initiateMPUrequest);

      return initiateMPUresponse.UploadId;
    }

    static List<string> UploadParts(string uploadID, AmazonGlacierClient client)
    {
      List<string> partChecksumList = new List<string>();
      long currentPosition = 0;
      var buffer = new byte[Convert.ToInt32(partSize)];

      long fileLength = new FileInfo(archiveToUpload).Length;
      using (FileStream fileToUpload = new FileStream(archiveToUpload, FileMode.Open, FileAccess.Read))
      {
        while (fileToUpload.Position < fileLength)
        {
          Stream uploadPartStream = GlacierUtils.CreatePartStream(fileToUpload, partSize);
          string checksum = TreeHashGenerator.CalculateTreeHash(uploadPartStream);
          partChecksumList.Add(checksum);
          // Upload part.
          UploadMultipartPartRequest uploadMPUrequest = new UploadMultipartPartRequest()
          {

            VaultName = vaultName,
            Body = uploadPartStream,
            Checksum = checksum,
            UploadId = uploadID
          };
          uploadMPUrequest.SetRange(currentPosition, currentPosition + uploadPartStream.Length - 1);
          client.UploadMultipartPart(uploadMPUrequest);

          currentPosition = currentPosition + uploadPartStream.Length;
        }
      }
      return partChecksumList;
    }

    static string CompleteMPU(string uploadID, AmazonGlacierClient client, List<string> partChecksumList)
    {
      long fileLength = new FileInfo(archiveToUpload).Length;
      CompleteMultipartUploadRequest completeMPUrequest = new CompleteMultipartUploadRequest()
      {
        UploadId = uploadID,
        ArchiveSize = fileLength.ToString(),
        Checksum = TreeHashGenerator.CalculateTreeHash(partChecksumList),
        VaultName = vaultName
      };

      CompleteMultipartUploadResponse completeMPUresponse = client.CompleteMultipartUpload(completeMPUrequest);
      return completeMPUresponse.ArchiveId;
    }
  }
}
```