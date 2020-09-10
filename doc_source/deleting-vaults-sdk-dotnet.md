# Deleting a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="deleting-vaults-sdk-dotnet"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for \.NET provide a method to delete a vault\.

**Topics**
+ [Deleting a Vault Using the High\-Level API of the AWS SDK for \.NET](#deleting-vault-sdk-dotnet-high-level)
+ [Deleting a Vault Using the Low\-Level API of the AWS SDK for \.NET](#deleting-vault-sdk-dotnet-low-level)

## Deleting a Vault Using the High\-Level API of the AWS SDK for \.NET<a name="deleting-vault-sdk-dotnet-high-level"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `DeleteVault` method you can use to delete a vault\.

### Example: Deleting a Vault Using the High\-Level API of the AWS SDK for \.NET<a name="deleting-vaults-sdk-dotnet-high-level-example"></a>

For a working code example, see [Example: Vault Operations Using the High\-Level API of the AWS SDK for \.NET ](creating-vaults-dotnet-sdk.md#vault-operations-example-dotnet-highlevel)\. The C\# code example shows basic vault operations including create and delete vault\. 

## Deleting a Vault Using the Low\-Level API of the AWS SDK for \.NET<a name="deleting-vault-sdk-dotnet-low-level"></a>

The following are the steps to delete a vault using the AWS SDK for \.NET\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region from where you want to delete a vault\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `DeleteVaultRequest` class\.

   You need to provide the vault name and account ID\. If you don't provide an account ID, then account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\. 

1. Run the `DeleteVault` method by providing the request object as a parameter\. 

   Amazon S3 Glacier \(S3 Glacier\) deletes the vault only if it is empty\. For more information, see [Delete Vault \(DELETE vault\)](api-vault-delete.md)\.

The following C\# code snippet illustrates the preceding steps\. The snippet retrieves metadata information of a vault that exists in the default AWS Region\. 

```
AmazonGlacier client;
client = new AmazonGlacierClient(Amazon.RegionEndpoint.USEast1);

DeleteVaultRequest request = new DeleteVaultRequest()
{
  VaultName = "*** provide vault name ***"
};

DeleteVaultResponse response = client.DeleteVault(request);
```

**Note**  
For information about the underlying REST API, see [Delete Vault \(DELETE vault\)](api-vault-delete.md)\.

### Example: Deleting a Vault Using the Low\-Level API of the AWS SDK for \.NET<a name="creating-vaults-sdk-dotnet-low-level-example"></a>

For a working code example, see [Example: Vault Operations Using the Low\-Level API of the AWS SDK for \.NET ](creating-vaults-dotnet-sdk.md#vault-operations-example-dotnet-lowlevel)\. The C\# code example shows basic vault operations including create and delete vault\. 