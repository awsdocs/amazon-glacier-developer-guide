# Step 2: Create a Vault in S3 Glacier<a name="getting-started-create-vault"></a>

A vault is a container for storing archives\. Your first step is to create a vault in one of the supported AWS Regions\. For a list of the AWS Regions that are supported by Amazon S3 Glacier, see [ Amazon S3 Glacier endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/glacier-service.html) in the *AWS General Reference*\.

You can create vaults programmatically or by using the S3 Glacier console\. This section uses the console to create a vault\.

**To create a vault**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/](https://console.aws.amazon.com/glacier/)\.

1. In the left navigation pane, choose **Vaults**\.

1. Choose **Create vault**\.

   The **Create vault** page opens\.

1. Under **Select a Region**, select an AWS Region from the Region selector\. Your vault will be located in the Region that you select\.

1. For **Vault name**, enter a name for your vault\.

   The following are the vault\-naming requirements:
   + A vault name must be unique within an AWS account and the AWS Region in which the vault is created\.
   + A vault name must be between 1 and 255 characters long\.
   + A vault name can contain only the following characters: **a–z**, **A–Z**, **0–9**, **\_** \(underscore\), **\-** \(hyphen\), and **\.** \(period\)\.

1. Under **Event notifications**, to turn on or off notifications on a vault for when a job is completed, choose one of the following settings:
   + **Turn off notifications** – Notifications are turned off, and notifications are not sent to an Amazon Simple Notification Service \(Amazon SNS\) topic when a specified job is completed\. 
   + **Turn on notifications** – Notifications are turned on, and notifications are sent to the provided Amazon SNS topic when a specified job is completed\. 

     If you chose **Turn on notifications**, see [Configuring Vault Notifications by Using the Amazon S3 Glacier Console](https://docs.aws.amazon.com/amazonglacier/latest/dev/configuring-notifications-console.html)\.

1. If the AWS Region and vault name are correct, then choose **Create vault**\. 

Your new vault is now listed on the **Vaults** page in the S3 Glacier console\.