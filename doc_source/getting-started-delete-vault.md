# Step 6: Delete a Vault in Amazon S3 Glacier<a name="getting-started-delete-vault"></a>

A vault is a container for storing archives\. To delete an Amazon S3 Glacier \(S3 Glacier\) vault you must first delete all existing archives in the vault as of the last inventory that S3 Glacier computed\.

You can delete a vault programmatically or by using the S3 Glacier console\. For information about deleting a vault programmatically, see [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)\.

**To delete an empty vault**

1. Sign into the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/](https://console.aws.amazon.com/glacier/)\.

1. From the AWS Region selector, select the AWS Region where the vault exists that you want to delete\.

   In this getting started exercise, we use the US West \(Oregon\) Region\.

1. Select the empty vault that you want to delete\. If the vault is not empty you must delete all achives before deleting the vault, see [Deleting an Archive in Amazon S3 Glacier](deleting-an-archive.md) 

   In this getting started exercise, we've been using a vault named **examplevault**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-list.png)

1. Click **Delete Vault**\. 

**To delete a nonempty vault**
+ If deleting a nonempty vault you must first delete all existing archives before deleting the vault\. This can be done by writing code to make a delete archive request using either the REST API, the AWS SDK for Java, the AWS SDK for \.NET or by using the AWS CLI\. For information about deleting archives, see [Step 5: Delete an Archive from a Vault in Amazon S3 Glacier](getting-started-delete-archive.md) 