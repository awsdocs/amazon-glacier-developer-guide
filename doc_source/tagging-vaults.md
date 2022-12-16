# Tagging Your S3 Glacier Vaults<a name="tagging-vaults"></a>

You can assign your own metadata to Amazon S3 Glacier vaults in the form of tags\. A *tag* is a key\-value pair that you define for a vault\. For basic information about tagging, including restrictions on tags, see [Tagging Amazon S3 Glacier Resources](tagging.md)\.

The following topics describe how you can add, list, and remove tags for vaults\.

**Topics**
+ [Tagging Vaults by Using the Amazon S3 Glacier Console](#tagging-console)
+ [Tagging Vaults by Using the AWS CLI](#tagging-cli)
+ [Tagging Vaults by Using the Amazon S3 Glacier API](#tagging-api)
+ [Related Sections](#related-sections-tagging-vaults)

## Tagging Vaults by Using the Amazon S3 Glacier Console<a name="tagging-console"></a>

You can add, list, and remove tags using the S3 Glacier console, as described in the following procedures\.

**To view the tags for a vault**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Under **Select a Region**, select an AWS Region from the Region selector\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, choose a vault\.

1. Choose the **Vaults properties** tab\. Scroll to the **Tags** section to view the tags associated with the vault\.

**To add a tag to a vault**

You can associate up to 50 tags to a vault\. Tags that are associated with a vault must have unique tag keys\.

For more information about tag restrictions, see [Tagging Amazon S3 Glacier Resources](https://docs.aws.amazon.com/amazonglacier/latest/dev/tagging.html)\.

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Under **Select a Region**, select an AWS Region from the Region selector\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, choose the name of the vault that you want to add tags to\. 

1. Choose the **Vault properties** tab\.

1. In the **Tags** section, choose **Add**\. The **Add tags** page appears\. 

1. On the **Add tags** page, specify the tag key in the **Key** field, and optionally specify a tag value in the **Value** field\. 

1. Choose **Save changes**\.

**To edit a tag**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Under **Select a Region**, select an AWS Region from the Region selector\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, choose a vault name\. 

1. Choose the **Vault properties** tab, and then scroll down to the **Tags** section\.

1. Under **Tags**, select the check box next to the tags that you want to change, then choose **Edit**\. The **Edit tags** page appears\. 

1. Update the tag key in the **Key** field, and optionally update the tag value in the **Value** field\.

1. Choose **Save changes**\.

**To remove a tag from a vault**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Under **Select a Region**, select an AWS Region from the Region selector\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, choose the name of the vault that you want to remove tags from\. 

1. Choose the **Vault properties** tab\. Scroll down to the **Tags** section\.

1. Under **Tags**, select the check box next to the tags that you want to remove, then choose **Delete**\. 

1. The **Delete tags** dialog box opens\. To confirm that you want to delete the selected tags, choose **Delete**\.

## Tagging Vaults by Using the AWS CLI<a name="tagging-cli"></a>

Follow these steps to add, list, or remove tags by using the AWS Command Line Interface \(AWS CLI\)\.

Each tag is composed of a key and a value\. Each vault can have up to 50 tags\.

1. To add tags to a vault, use the `add-tags-to-vault` command\. 

   ```
   aws glacier add-tags-to-vault --vault-name examplevault --account-id 111122223333 --tags id=1234,date=2020
   ```

    For more information on this vault operation, see [Add Tags To Vault ](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-AddTagsToVault.html)\.

1. To list all the tags attached to a vault, use the `list-tags-for-vault` command\.

   ```
   aws glacier list-tags-for-vault --vault-name examplevault --account-id 111122223333
   ```

    For more information on this vault operation, see [List Tags For Vault](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-ListTagsForVault.html)\.

1. To remove one or more tags from the set of tags attached to a vault, use the `remove-tags-from-vault` command\.

   ```
   aws glacier remove-tags-from-vault --vault-name examplevault --account-id 111122223333 --tag-keys date
   ```

   For more information on this vault operation, see [Remove Tags From Vault](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-RemoveTagsFromVault.html)\.

## Tagging Vaults by Using the Amazon S3 Glacier API<a name="tagging-api"></a>

You can add, list, and remove tags by using the S3 Glacier API\. For examples, see the following documentation:

 [Add Tags To Vault \(POST tags add\)](api-AddTagsToVault.md)   
Adds or updates tags for the specified vault\.

 [List Tags For Vault \(GET tags\)](api-ListTagsForVault.md)   
Lists the tags for the specified vault\.

 [Remove Tags From Vault \(POST tags remove\)](api-RemoveTagsFromVault.md)   
Removes tags from the specified vault\.

## Related Sections<a name="related-sections-tagging-vaults"></a>

 
+ [Tagging Amazon S3 Glacier Resources](tagging.md)