# Creating a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="creating-vaults-dotnet-sdk"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to create a vault\.

**Topics**
+ [Creating a Vault Using the High\-Level API of the AWS SDK for \.NET](#create-vault-dotnet-highlevel)
+ [Creating a Vault Using the Low\-Level API of the AWS SDK for \.NET](#create-vault-dotnet-lowlevel)

## Creating a Vault Using the High\-Level API of the AWS SDK for \.NET<a name="create-vault-dotnet-highlevel"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `CreateVault` method you can use to create a vault in an AWS Region\.

### Example: Vault Operations Using the High\-Level API of the AWS SDK for \.NET<a name="vault-operations-example-dotnet-highlevel"></a>

The following C\# code example creates and delete a vault in the US West \(Oregon\) Region\. For a list of AWS Regions in which you can create vaults, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\. 

For step\-by\-step instructions on how to run the following example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with a vault name\. 

**Example**  

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Transfer;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class VaultCreateDescribeListVaultsDeleteHighLevel
  {
    static string vaultName = "*** Provide vault name ***";

    public static void Main(string[] args)
    {
      try
      {
          var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
          manager.CreateVault(vaultName);
          Console.WriteLine("Vault created. To delete the vault, press Enter");
          Console.ReadKey();
          manager.DeleteVault(vaultName);
          Console.WriteLine("\nVault deleted. To continue, press Enter");
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

## Creating a Vault Using the Low\-Level API of the AWS SDK for \.NET<a name="create-vault-dotnet-lowlevel"></a>

The low\-level API provides methods for all the vault operations, including create and delete vaults, get a vault description, and get a list of vaults created in a specific AWS Region\. The following are the steps to create a vault using the AWS SDK for \.NET\. 

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region in which you want to create a vault\. All operations you perform using this client apply to that AWS Region\.

1. Provide request information by creating an instance of the `CreateVaultRequest` class\.

    Amazon S3 Glacier \(S3 Glacier\) requires you to provide a vault name and your account ID\. If you don't provide an account ID, then account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\. 

1. Run the `CreateVault` method by providing the request object as a parameter\. 

   The response S3 Glacier returns is available in the `CreateVaultResponse` object\.

### Example: Vault Operations Using the Low\-Level API of the AWS SDK for \.NET<a name="vault-operations-example-dotnet-lowlevel"></a>

The following C\# example illustrates the preceding steps\. The example creates a vault in the US West \(Oregon\) Region\. In addition, the code example retrieves the vault information, lists all vaults in the same AWS Region, and then deletes the vault created\. The `Location` printed is the relative URI of the vault that includes your account ID, the AWS Region, and the vault name\.

**Note**  
For information about the underlying REST API, see [Create Vault \(PUT vault\)](api-vault-put.md)\. 

For step\-by\-step instructions on how to run the following example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown with a vault name\. 

**Example**  

```
using System;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class VaultCreateDescribeListVaultsDelete
  {
    static string vaultName = "*** Provide vault name ***";
    static AmazonGlacierClient client;

    public static void Main(string[] args)
    {
       try
      {
         using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2))
        {
          Console.WriteLine("Creating a vault.");
          CreateAVault();
          DescribeVault();
          GetVaultsList();
          Console.WriteLine("\nVault created. Now press Enter to delete the vault...");
          Console.ReadKey();
          DeleteVault();
        }
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      Console.WriteLine("To continue, press Enter");
      Console.ReadKey();
    }

    static void CreateAVault()
    {
      CreateVaultRequest request = new CreateVaultRequest()
      {
        VaultName = vaultName
      };
      CreateVaultResponse response = client.CreateVault(request);
      Console.WriteLine("Vault created: {0}\n", response.Location); 
    }

    static void DescribeVault()
    {
      DescribeVaultRequest describeVaultRequest = new DescribeVaultRequest()
      {
        VaultName = vaultName
      };
   
      DescribeVaultResponse describeVaultResponse = client.DescribeVault(describeVaultRequest);
      Console.WriteLine("\nVault description...");
      Console.WriteLine(
        "\nVaultName: " + describeVaultResponse.VaultName +
        "\nVaultARN: " + describeVaultResponse.VaultARN +
        "\nVaultCreationDate: " + describeVaultResponse.CreationDate +
        "\nNumberOfArchives: " + describeVaultResponse.NumberOfArchives +
        "\nSizeInBytes: " + describeVaultResponse.SizeInBytes +
        "\nLastInventoryDate: " + describeVaultResponse.LastInventoryDate 
        );
    }

    static void GetVaultsList()
    {
      string lastMarker = null;
      Console.WriteLine("\n List of vaults in your account in the specific region ...");
      do
      {
        ListVaultsRequest request = new ListVaultsRequest()
        {
          Marker = lastMarker
        };
        ListVaultsResponse response = client.ListVaults(request);
         
        foreach (DescribeVaultOutput output in response.VaultList)
        {
          Console.WriteLine("Vault Name: {0} \tCreation Date: {1} \t #of archives: {2}",
                            output.VaultName, output.CreationDate, output.NumberOfArchives); 
        }
        lastMarker = response.Marker;
      } while (lastMarker != null);
    }

    static void DeleteVault()
    {
      DeleteVaultRequest request = new DeleteVaultRequest()
      {
        VaultName = vaultName
      };
      DeleteVaultResponse response = client.DeleteVault(request);
    }
  }
}
```