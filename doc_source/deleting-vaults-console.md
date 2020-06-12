# Deleting an Empty Vault Using the Amazon S3 Glacier Console<a name="deleting-vaults-console"></a>

Amazon S3 Glacier \(S3 Glacier\) deletes a vault only if there are no archives in the vault as of the last inventory it computed and there have been no writes to the vault since the last inventory\. For information about deleting archives, see [Deleting an Archive in Amazon S3 Glacier](deleting-an-archive.md)\. For information about downloading a vault inventory, [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)\. 

The following are the steps to delete an empty vault using the S3 Glacier console\.

1. Sign into the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. From the AWS Region selector, select the AWS Region where the vault exists\.

1. Select the vault\. 

1. Click **Delete Vault**\. 