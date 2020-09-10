# Deleting an Archive in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="deleting-an-archive-using-dot-net"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to delete an archive\.

**Topics**
+ [Deleting an Archive Using the High\-Level API of the AWS SDK for \.NET](#delete-archive-using-dot-net-high-level)
+ [Deleting an Archive Using the Low\-Level API AWS SDK for \.NET](#delete-archive-using-dot-net-low-level)

## Deleting an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="delete-archive-using-dot-net-high-level"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `DeleteArchive` method you can use to delete an archive\. 

### Example: Deleting an Archive Using the High\-Level API of the AWS SDK for \.NET<a name="delete-archive-dot-net-high-level-example"></a>

The following C\# code example uses the high\-level API of the AWS SDK for \.NET to delete an archive\. For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the archive ID of the archive you want to delete\.

**Example**  

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime; 

namespace glacier.amazon.com.docsamples
{
  class ArchiveDeleteHighLevel
  {
    static string vaultName = "examplevault";
    static string archiveId = "*** Provide archive ID ***";

    public static void Main(string[] args)
    {
      try
      {
        var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
        manager.DeleteArchive(vaultName, archiveId);
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

## Deleting an Archive Using the Low\-Level API AWS SDK for \.NET<a name="delete-archive-using-dot-net-low-level"></a>

The following are the steps to delete an using the AWS SDK for \.NET\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the archive you want to delete is stored\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `DeleteArchiveRequest` class\.

   You need to provide an archive ID, a vault name, and your account ID\. If you don't provide an account ID, then account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)\.

1. Run the `DeleteArchive` method by providing the request object as a parameter\. 

### Example: Deleting an Archive Using the Low\-Level API of the AWS SDK for \.NET<a name="delete-archive-dot-net-low-level-example"></a>

The following C\# example illustrates the preceding steps\. The example uses the low\-level API of the AWS SDK for \.NET to delete an archive\.

**Note**  
For information about the underlying REST API, see [Delete Archive \(DELETE archive\)](api-archive-delete.md)\.

 For step\-by\-step instructions on how to run this example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with the archive ID of the archive you want to delete\.

**Example**  

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class ArchiveDeleteLowLevel
  {
    static string vaultName = "examplevault";
    static string archiveId = "*** Provide archive ID ***";

    public static void Main(string[] args)
    {
      AmazonGlacierClient client;
      try
      {
        using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2))
        {
          Console.WriteLine("Deleting the archive");
          DeleteAnArchive(client);
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

    static void DeleteAnArchive(AmazonGlacierClient client)
    {
      DeleteArchiveRequest request = new DeleteArchiveRequest()
      {
        VaultName = vaultName,
        ArchiveId = archiveId
      };
      DeleteArchiveResponse response = client.DeleteArchive(request);
    }
  }
}
```