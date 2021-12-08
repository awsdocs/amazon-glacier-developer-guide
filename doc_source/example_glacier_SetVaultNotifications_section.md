# Set Amazon S3 Glacier vault notifications using an AWS SDK<a name="example_glacier_SetVaultNotifications_section"></a>

The following code example shows how to set Amazon S3 Glacier vault notifications\.

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
  

```
class GlacierWrapper:
    """Encapsulates Amazon S3 Glacier API operations."""
    def __init__(self, glacier_resource):
        """
        :param glacier_resource: A Boto3 Amazon S3 Glacier resource.
        """
        self.glacier_resource = glacier_resource


    def set_notifications(self, vault, sns_topic_arn):
        """
        Sets an Amazon Simple Notification Service (Amazon SNS) topic as a target
        for notifications. Amazon S3 Glacier publishes messages to this topic for
        the configured list of events.

        :param vault: The vault to set up to publish notifications.
        :param sns_topic_arn: The Amazon Resource Name (ARN) of the topic that
                              receives notifications.
        :return: Data about the new notification configuration.
        """
        try:
            notification = self.glacier_resource.Notification('-', vault.name)
            notification.set(vaultNotificationConfig={
                'SNSTopic': sns_topic_arn,
                'Events': ['ArchiveRetrievalCompleted', 'InventoryRetrievalCompleted']
            })
            logger.info(
                "Notifications will be sent to %s for events %s from %s.",
                notification.sns_topic, notification.events, notification.vault_name)
        except ClientError:
            logger.exception(
                "Couldn't set notifications to %s on %s.", sns_topic_arn, vault.name)
            raise
        else:
            return notification
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/glacier#code-examples)\. 
+  For API details, see [SetVaultNotifications](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/SetVaultNotifications) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, including help getting started and information about previous versions, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\.