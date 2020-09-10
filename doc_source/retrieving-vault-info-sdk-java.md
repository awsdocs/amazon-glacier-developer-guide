# Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS SDK for Java<a name="retrieving-vault-info-sdk-java"></a>

**Topics**
+ [Retrieve Vault Metadata for a Vault](#retrieve-vault-info-sdk-java-lowlevel-one-vault)
+ [Retrieve Vault Metadata for All Vaults in a Region](#retrieve-vault-info-sdk-java-lowlevel-all-vaults)
+ [Example: Retrieving Vault Metadata Using the AWS SDK for Java](#retrieving-vault-info-sdk-java-example)

## Retrieve Vault Metadata for a Vault<a name="retrieve-vault-info-sdk-java-lowlevel-one-vault"></a>

You can retrieve metadata for a specific vault or all the vaults in a specific AWS Region\. The following are the steps to retrieve vault metadata for a specific vault using the low\-level API of the AWS SDK for Java\. 

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the vault resides\. All operations you perform using this client apply to that AWS Region\.

1. Provide request information by creating an instance of the `DescribeVaultRequest` class\.

   Amazon S3 Glacier \(S3 Glacier\) requires you to provide a vault name and your account ID\. If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\.

1. Run the `describeVault` method by providing the request object as a parameter\. 

   The vault metadata information that S3 Glacier returns is available in the `DescribeVaultResult` object\.

The following Java code snippet illustrates the preceding steps\. 

```
DescribeVaultRequest request = new DescribeVaultRequest()
	.withVaultName("*** provide vault name***");

DescribeVaultResult result = client.describeVault(request);

System.out.print(
        "\nCreationDate: " + result.getCreationDate() +
        "\nLastInventoryDate: " + result.getLastInventoryDate() +
        "\nNumberOfArchives: " + result.getNumberOfArchives() + 
        "\nSizeInBytes: " + result.getSizeInBytes() + 
        "\nVaultARN: " + result.getVaultARN() + 
        "\nVaultName: " + result.getVaultName());
```

**Note**  
For information about the underlying REST API, see [Describe Vault \(GET vault\)](api-vault-get.md)\. 

## Retrieve Vault Metadata for All Vaults in a Region<a name="retrieve-vault-info-sdk-java-lowlevel-all-vaults"></a>

You can also use the `listVaults` method to retrieve metadata for all the vaults in a specific AWS Region\. 

The following Java code snippet retrieves list of vaults in the `us-west-2` Region\. The request limits the number of vaults returned in the response to 5\. The code snippet then makes a series of `listVaults` calls to retrieve the entire vault list from the AWS Region\. 

```
AmazonGlacierClient client;
client.setEndpoint("https://glacier.us-west-2.amazonaws.com/");

String marker = null;
do {            
    ListVaultsRequest request = new ListVaultsRequest()
        .withLimit("5")
        .withMarker(marker);
    ListVaultsResult listVaultsResult = client.listVaults(request);
    
    List<DescribeVaultOutput> vaultList = listVaultsResult.getVaultList();
    marker = listVaultsResult.getMarker();
    for (DescribeVaultOutput vault : vaultList) {
        System.out.println(
                "\nCreationDate: " + vault.getCreationDate() +
                "\nLastInventoryDate: " + vault.getLastInventoryDate() +
                "\nNumberOfArchives: " + vault.getNumberOfArchives() + 
                "\nSizeInBytes: " + vault.getSizeInBytes() + 
                "\nVaultARN: " + vault.getVaultARN() + 
                "\nVaultName: " + vault.getVaultName()); 
    }
} while (marker != null);
```

In the preceding code segment, if you don't specify the `Limit` value in the request, S3 Glacier returns up to 10 vaults, as set by the S3 Glacier API\. If there are more vaults to list, the response `marker` field contains the vault Amazon Resource Name \(ARN\) at which to continue the list with a new request; otherwise, the `marker` field is null\. 

Note that the information returned for each vault in the list is the same as the information you get by calling the `describeVault` method for a specific vault\. 

**Note**  
The `listVaults` method calls the underlying REST API \(see [List Vaults \(GET vaults\)](api-vaults-get.md)\)\. 

## Example: Retrieving Vault Metadata Using the AWS SDK for Java<a name="retrieving-vault-info-sdk-java-example"></a>

For a working code example, see [Example: Creating a Vault Using the AWS SDK for Java](creating-vaults-sdk-java.md#creating-vaults-sdk-java-example)\. The Java code example creates a vault and retrieves the vault metadata\.