# Step 6: Delete a Vault in Amazon S3 Glacier<a name="getting-started-delete-vault"></a>

A vault is a container for storing archives\. You can delete an Amazon S3 Glacier \(Glacier\) vault only if there are no archives in the vault as of the last inventory that Glacier computed and there have been no writes to the vault since the last inventory\. 

**Note**  
Glacier prepares an inventory for each vault periodically, every 24 hours\. Because the inventory might not reflect the latest information, Glacier ensures the vault is indeed empty by checking if there were any write operations since the last vault inventory\.

You can delete a vault programmatically or by using the Glacier console\. This section uses the console to delete a vault\. For information about deleting a vault programmatically, see [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)\.

**To delete a vault**

1. Sign into the AWS Management Console and open the Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. From the region selector, select the AWS region where the vault exists that you want to delete\.

   In this getting started exercise, we use the US West \(Oregon\) region\.

1. Select the vault that you want to delete\. 

   In this getting started exercise, we've been using a vault named **examplevault**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-list.png)

1. Click **Delete Vault**\. 