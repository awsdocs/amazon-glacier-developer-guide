# Deleting a Vault in Amazon Glacier<a name="deleting-vaults"></a>

Amazon Glacier deletes a vault only if there are no archives in the vault as of the last inventory it computed and there have been no writes to the vault since the last inventory\. For information about deleting archives, see [Deleting an Archive in Amazon Glacier](deleting-an-archive.md)\. For information about downloading a vault inventory, [Downloading a Vault Inventory in Amazon Glacier](vault-inventory.md)\. 

**Note**  
Amazon Glacier prepares an inventory for each vault periodically, every 24 hours\. Because the inventory might not reflect the latest information, Amazon Glacier ensures the vault is indeed empty by checking if there were any write operations since the last vault inventory\. 

**Topics**
+ [Deleting a Vault in Amazon Glacier Using the AWS SDK for Java](deleting-vaults-sdk-java.md)
+ [Deleting a Vault in Amazon Glacier Using the AWS SDK for \.NET](deleting-vaults-sdk-dotnet.md)
+ [Deleting a Vault in Amazon Glacier Using the REST API](deleting-vault-rest-api.md)
+ [Deleting a Vault Using the Amazon Glacier Console](deleting-vaults-console.md)