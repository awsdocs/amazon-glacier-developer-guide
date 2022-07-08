# Deleting an Empty Vault by Using the S3 Glacier Console<a name="deleting-vaults-console"></a>

**Note**  
Before deleting a vault, you must delete all existing archives within the vault\. You can do this by writing code to make a delete archive request by using either the REST API, the AWS SDK for Java, the AWS SDK for \.NET, or by using the AWS Command Line Interface \(AWS CLI\)\. For information about deleting archives, see [Step 5: Delete an Archive from a Vault in S3 Glacier](getting-started-delete-archive.md)\.

After your vault is empty, you can delete it by using the following steps\.

**To delete an empty vault by using the Amazon S3 Glacier console**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Under **Select a Region**, choose the AWS Region where the vault exists\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, select the option button next to the name of the vault that you want to delete, and then choose **Delete** at the top of the page\.

1. In the **Delete vault** dialog box, confirm that you want to delete the vault by choosing **Delete**\. 
**Important**  
Deleting a vault can't be undone\.

1. To verify that you've deleted the vault, open the **Vaults** list and enter the name of the vault that you deleted\. If the vault can't be found, your deletion was successful\. 