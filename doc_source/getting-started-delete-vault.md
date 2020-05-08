# Step 6: Delete a Vault in Amazon S3 Glacier<a name="getting-started-delete-vault"></a>

A vault is a container for storing archives\. You can delete an Amazon S3 Glacier \(S3 Glacier\) vault only if there are no archives in the vault as of the last inventory that S3 Glacier computed and there have been no writes to the vault since the last inventory\. For information about deleting archives, see [Deleting an Archive in Amazon S3 Glacier](deleting-an-archive.md)

**Note**  
S3 Glacier prepares an inventory for each vault periodically, every 24 hours\. Because the inventory might not reflect the latest information, S3 Glacier ensures the vault is indeed empty by checking if there were any write operations since the last vault inventory\.

You can delete a vault programmatically or by using the S3 Glacier console\. This section uses the console to delete a vault\. For information about deleting a vault programmatically, see [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)\.

**To delete a vault**

1. Sign into the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/](https://console.aws.amazon.com/glacier/)\.

1. From the AWS Region selector, select the AWS Region where the vault exists that you want to delete\.

   In this getting started exercise, we use the US West \(Oregon\) Region\.

1. Select the empty vault that you want to delete\. If the vault is not empty you must delete all achives before deleting the vault, see [Deleting an Archive in Amazon S3 Glacier](deleting-an-archive.md) 

   In this getting started exercise, we've been using a vault named **examplevault**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-list.png)

1. Click **Delete Vault**\. 