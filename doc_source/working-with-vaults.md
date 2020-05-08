# Working with Vaults in Amazon S3 Glacier<a name="working-with-vaults"></a>

A vault is a container for storing archives\. When you create a vault, you specify a vault name and the AWS Region in which you want to create the vault\. For a list of supported AWS Regions, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\. 

You can store an unlimited number of archives in a vault\. 

**Important**  
Amazon S3 Glacier \(S3 Glacier\) provides a management console\. You can use the console to create and delete vaults\. However, all other interactions with S3 Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using S3 Glacier with the AWS CLI, go to [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

**Topics**
+ [Vault Operations in S3 Glacier](#vault-operations-quick-intro)
+ [Creating a Vault in Amazon S3 Glacier](creating-vaults.md)
+ [Retrieving Vault Metadata in Amazon S3 Glacier](retrieving-vault-info.md)
+ [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)
+ [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)
+ [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)
+ [Tagging Your Amazon S3 Glacier Vaults](tagging-vaults.md)
+ [Amazon S3 Glacier Vault Lock](vault-lock.md)

## Vault Operations in S3 Glacier<a name="vault-operations-quick-intro"></a>

S3 Glacier supports various vault operations\. Vault operations are specific to particular AWS Regions\. In other words, when you create a vault, you create it in a specific AWS Region\. When you list vaults, S3 Glacier returns the vault list from the AWS Region you specified in the request\.

### Creating and Deleting Vaults<a name="vault-operations-create-delete-quick-intro"></a>

An AWS account can create up to 1,000 vaults per AWS Region\. For a list of the AWS Regions supported by S3 Glacier, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#glacier_region) in the *AWS General Reference*\.

You can delete a vault only if there are no archives in the vault as of the last inventory that S3 Glacier computed and there have been no writes to the vault since the last inventory\. 

**Note**  
S3 Glacier prepares an inventory for each vault periodically, every 24 hours\. Because the inventory might not reflect the latest information, S3 Glacier ensures the vault is indeed empty by checking if there were any write operations since the last vault inventory\. 

For more information, see [Creating a Vault in Amazon S3 Glacier](creating-vaults.md) and [Deleting a Vault in Amazon S3 Glacier](deleting-vaults.md)\. 

### Retrieving Vault Metadata<a name="vault-operations-retrieving-info-quick-intro"></a>

You can retrieve vault information such as the vault creation date, number of archives in the vault, and the total size of all the archives in the vault\. S3 Glacier provides API calls for you to retrieve this information for a specific vault or all the vaults in a specific AWS Region in your account\. For more information, see [Retrieving Vault Metadata in Amazon S3 Glacier](retrieving-vault-info.md)\.

### Downloading a Vault Inventory<a name="vault-operations-retrieving-inventory-quick-intro"></a>

A vault inventory refers to the list of archives in a vault\. For each archive in the list, the inventory provides archive information such as archive ID, creation date, and size\. S3 Glacier updates the vault inventory approximately once a day, starting on the day the first archive is uploaded to the vault\. A vault inventory must exist for you to be able to download it\.

Downloading a vault inventory is an asynchronous operation\. You must first initiate a job to download the inventory\. After receiving the job request, S3 Glacier prepares your inventory for download\. After the job completes, you can download the inventory data\. 

Given the asynchronous nature of the job, you can use Amazon Simple Notification Service \(Amazon SNS\) notifications to notify you when the job completes\. You can specify an Amazon SNS topic for each individual job request or configure your vault to send a notification when specific vault events occur\.

S3 Glacier prepares an inventory for each vault periodically, every 24 hours\. If there have been no archive additions or deletions to the vault since the last inventory, the inventory date is not updated\. When you initiate a job for a vault inventory, S3 Glacier returns the last inventory it generated, which is a point\-in\-time snapshot and not real\-time data\. You might not find it useful to retrieve vault inventory for each archive upload\. However, suppose you maintain a database on the client\-side associating metadata about the archives you upload to S3 Glacier\. Then, you might find the vault inventory useful to reconcile information in your database with the actual vault inventory\.

For more information about retrieving a vault inventory, see [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)\.

### Configuring Vault Notifications<a name="vault-operations-configure-notifications-quick-intro"></a>

Retrieving anything from S3 Glacier, such as an archive from a vault or a vault inventory, is a two\-step process in which you first initiate a job\. After the job completes, you can download the output\. You can use S3 Glacier notifications support to know when your job is complete\. S3 Glacier sends notification messages to an Amazon Simple Notification Service \(Amazon SNS\) topic that you provide\. 

You can configure notifications on a vault and identify vault events and the Amazon SNS topic to be notified when the event occurs\. Anytime the vault event occurs, S3 Glacier sends a notification to the specified Amazon SNS topic\. For more information, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\.