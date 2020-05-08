# Configuring Vault Notifications Using the Amazon S3 Glacier Console<a name="configuring-notifications-console"></a>

This section describes how to configure vault notifications using the Amazon S3 Glacier \(S3 Glacier\) console\. When you configure notifications, you specify job completion events that trigger notification to an Amazon Simple Notification Service \(Amazon SNS\) topic\. In addition to configuring notifications for the vault, you can also specify a topic to publish notification to when you initiate a job\. If your vault is configured to notify for a specific event and you specify notification in the job initiation request, then two notifications are sent\. 

**To configure a vault notification**

1. Sign into the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier](https://console.aws.amazon.com/glacier)\.

1. Select a vault in the vault list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/EnableNotifications05.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. Select the **Notifications** tab\.

1. Select the **enabled** in the **Notifications** field\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/EnableNotifications10.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. On the **Notifications** tab, do one of the following:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/configuring-notifications-console.html)

1. Select the events that trigger notification\.

   For example, to trigger notification when only archive retrieval jobs are complete, check only **Get Archive Job Complete**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/EnableNotifications30.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. Click **Save**\.
**Important**  
By default, a new topic does not have any subscriptions associated with it\. To receive notifications published to this topic, you must subscribe to the topic\. Follow the steps in [Subscribe to a Topic](https://docs.aws.amazon.com/sns/latest/gsg/Subscribe.html) in the *Amazon Simple Notification Service Getting Started Guide* to subscribe to a new topic\.