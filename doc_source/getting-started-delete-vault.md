# Step 6: Delete a Vault in S3 Glacier<a name="getting-started-delete-vault"></a>

A vault is a container for storing archives\. To delete an Amazon S3 Glacier vault, you must first delete all existing archives in the vault as of the last inventory that S3 Glacier computed\.

You can delete a vault programmatically or by using the S3 Glacier console\. For information about deleting a vault programmatically, see [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)\.

**To delete an empty vault**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/](https://console.aws.amazon.com/glacier/)\.

1. From the **Select a Region** menu, choose the AWS Region for the vault that you want to delete\.

   In this getting started exercise, your example vault is in the US West \(Oregon\) Region\.

1. Select the option button next to the empty vault that you want to delete\. If the vault is not empty, you must delete all archives before deleting the vault\. For more information, see [Deleting an Archive in Amazon S3 Glacier](deleting-an-archive.md)\. 
**Important**  
Deleting a vault can't be undone\. 

1. Choose **Delete**\. 

1. The **Delete vault** dialog box appears\. Choose **Delete**\.

**To delete a nonempty vault**

1. If you're deleting a nonempty vault, you must first delete all existing archives before deleting the vault\. You can do this by writing code to make a delete archive request by using either the REST API, the AWS SDK for Java, the AWS SDK for \.NET or the AWS CLI\. For information about deleting archives, see [Step 5: Delete an Archive from a Vault in S3 Glacier](getting-started-delete-archive.md)\. 

1. After the vault is empty, follow the steps to delete an empty vault in the preceding procedure\. 