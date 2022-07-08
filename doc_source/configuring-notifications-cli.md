# Configuring Vault Notifications Using the AWS Command Line Interface<a name="configuring-notifications-cli"></a>

This section describes how to configure vault notifications using the AWS Command Line Interface\. When you configure notifications, you specify job completion events that trigger notification to an Amazon Simple Notification Service \(Amazon SNS\) topic\. In addition to configuring notifications for the vault, you can also specify a topic to publish notification to when you initiate a job\. If your vault is configured to notify for a specific event and you specify notification in the job initiation request, then two notifications are sent\. 

Follow these steps to configure vault notification using the AWS CLI\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Configure Vault Notifications Using the AWS CLI](#Configure-Vault-Notifications-CLI-Implementation)

## \(Prerequisite\) Setting Up the AWS CLI<a name="Creating-Vaults-CLI-Setup"></a>

1. Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*\. 

    [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 

   [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

1. Verify the setup by entering the following commands at the command prompt\. These commands don't provide credentials explicitly, so the credentials of the default profile are used\.
   + Try using the help command\.

     ```
     aws help
     ```
   + Use `aws s3 ls` to get a list of buckets on the configured account\.

     ```
     aws s3 ls
     ```
   + Use `aws configure list` to see the current configuration data\.

     ```
     aws configure list
     ```

## Example: Configure Vault Notifications Using the AWS CLI<a name="Configure-Vault-Notifications-CLI-Implementation"></a>

1. Use the `set-vault-notifications` command to configure notifications that will be sent when specific events happen to a vault\. By default, you don't get any notifications\.

   ```
   aws glacier set-vault-notifications --vault-name examplevault --account-id 111122223333 --vault-notification-config file://notificationconfig.json
   ```

1.  The notification configuration is a JSON document as shown in the following example\. 

   ```
   {    
      "SNSTopic": "arn:aws:sns:us-west-2:012345678901:mytopic",    
      "Events": ["ArchiveRetrievalCompleted", "InventoryRetrievalCompleted"] 
   }
   ```

   For more information about using Amazon SNS topics for Glacier see, [Configuring Vault Notifications in S3 Glacier: General Concepts](configuring-notifications.html#configuring-notifications.general)

   For more information about Amazon SNS, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/gsg/Welcome.html)\.