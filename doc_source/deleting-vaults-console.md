# Deleting a Vault Using the Amazon Glacier Console<a name="deleting-vaults-console"></a>

Amazon Glacier deletes a vault only if there are no archives in the vault as of the last inventory it computed and there have been no writes to the vault since the last inventory\. For information about deleting archives, see [Deleting an Archive in Amazon Glacier](deleting-an-archive.md)\. For information about downloading a vault inventory, [Downloading a Vault Inventory in Amazon Glacier](vault-inventory.md)\. 

The following are the steps to delete a vault using the Amazon Glacier console\.

1. Sign into the AWS Management Console and open the Amazon Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. From the region selector, select the AWS region where the vault exists\.

1. Select the vault\. 

1. Click **Delete Vault**\. 