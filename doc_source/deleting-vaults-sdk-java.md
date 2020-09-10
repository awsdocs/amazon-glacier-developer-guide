# Deleting a Vault in Amazon S3 Glacier Using the AWS SDK for Java<a name="deleting-vaults-sdk-java"></a>

The following are the steps to delete a vault using the low\-level API of the AWS SDK for Java\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region from where you want to delete a vault\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `DeleteVaultRequest` class\.

   You need to provide the vault name and account ID\. If you don't provide an account ID, then account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\. 

1. Run the `deleteVault` method by providing the request object as a parameter\. 

   Amazon S3 Glacier \(S3 Glacier\) deletes the vault only if it is empty\. For more information, see [Delete Vault \(DELETE vault\)](api-vault-delete.md)\.

The following Java code snippet illustrates the preceding steps\. 

```
try {
    DeleteVaultRequest request = new DeleteVaultRequest()
        .withVaultName("*** provide vault name ***");

    client.deleteVault(request);
    System.out.println("Deleted vault: " + vaultName);
} catch (Exception e) {
    System.err.println(e.getMessage());
}
```

**Note**  
For information about the underlying REST API, see [Delete Vault \(DELETE vault\)](api-vault-delete.md)\. 

## Example: Deleting a Vault Using the AWS SDK for Java<a name="deleting-vaults-sdk-java-example"></a>

For a working code example, see [Example: Creating a Vault Using the AWS SDK for Java](creating-vaults-sdk-java.md#creating-vaults-sdk-java-example)\. The Java code example shows basic vault operations including create and delete vault\. 