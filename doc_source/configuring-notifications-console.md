# Configuring Vault Notifications by Using the S3 Glacier Console<a name="configuring-notifications-console"></a>

This section describes how to configure vault notifications by using the Amazon S3 Glacier console\. When you configure notifications, you specify job\-completion events that send a notification to an Amazon Simple Notification Service \(Amazon SNS\) topic\. In addition to configuring notifications for the vault, you can also specify a topic to publish notifications to when you initiate a job\. If your vault is configured to send a notification for a specific event and you also configure notifications in the job\-initiation request, then two notifications are sent\. 

**To configure a vault notification**

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/home](https://console.aws.amazon.com/glacier/home)\.

1. In the left navigation pane, choose **Vaults**\.

1. In the **Vaults** list, choose a vault\.

1. In the **Notifications** section, choose **Edit**\.

1. On the **Event notifications** page, choose **Turn on notifications**\.

1. In the **Notifications** section, choose one of the following Amazon Simple Notification Service \(Amazon SNS\) options, and then follow the corresponding steps:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/configuring-notifications-console.html)

1. Under **Events**, select one or both events that you want to send notifications:
   + To send a notification only when archive retrieval jobs are complete, select **Archive Retrieval Job Complete**\. 
   + To send a notification only when vault inventory jobs are complete, select **Vault Inventory Retrieval Job Complete**\. 