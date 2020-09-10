# Configuring Vault Notifications in Amazon S3 Glacier<a name="configuring-notifications"></a>

Retrieving anything from Amazon S3 Glacier \(S3 Glacier\), such as an archive from a vault or a vault inventory, is a two\-step process\. 

1. Initiate a retrieval job\. 

1. After the job completes, download the job output\. 

You can set a notification configuration on a vault so that when a job completes a message is sent to an Amazon Simple Notification Service \(Amazon SNS\) topic\. 

**Topics**
+ [Configuring Vault Notifications in S3 Glacier: General Concepts](#configuring-notifications.general)
+ [Configuring Vault Notifications in Amazon S3 Glacier Using the AWS SDK for Java](configuring-notifications-sdk-java.md)
+ [Configuring Vault Notifications in Amazon S3 Glacier Using the AWS SDK for \.NET](configuring-notifications-sdk-dotnet.md)
+ [Configuring Vault Notifications in S3 Glacier Using the REST API](configuring-notifications-rest-api.md)
+ [Configuring Vault Notifications Using the Amazon S3 Glacier Console](configuring-notifications-console.md)

## Configuring Vault Notifications in S3 Glacier: General Concepts<a name="configuring-notifications.general"></a>

A S3 Glacier retrieval job request is ran asynchronously\. You must wait until S3 Glacier completes the job before you can get its output\. You can periodically poll S3 Glacier to determine the job status, but that is not an optimal approach\. S3 Glacier also supports notifications\. When a job completes, it can post a message to an Amazon Simple Notification Service \(Amazon SNS\) topic\. This requires you to set notification configuration on the vault\. In the configuration, you identify one or more events and an Amazon SNS topic to which you want S3 Glacier to send a message when the event occurs\. 

S3 Glacier defines events specifically related to job completion \(`ArchiveRetrievalCompleted`, `InventoryRetrievalCompleted`\) that you can add to the vault's notification configuration\. When a specific job completes, S3 Glacier publishes a notification message to the SNS topic\.

 The notification configuration is a JSON document as shown in the following example\. 

```
{    
   "Topic": "arn:aws:sns:us-west-2:012345678901:mytopic",    
   "Events": ["ArchiveRetrievalCompleted", "InventoryRetrievalCompleted"] 
}
```

Note that you can configure only one Amazon SNS topic for a vault\. 

**Note**  
Adding a notification configuration to a vault causes S3 Glacier to send a notification each time the event specified in the notification configuration occurs\. You can also optionally specify an Amazon SNS topic in each job initiation request\. If you add both the notification configuration on the vault and also specify an Amazon SNS topic in your initiate job request, S3 Glacier sends both notifications\. 

The job completion message S3 Glacier sends include information such as the type of job \(`InventoryRetrieval`, `ArchiveRetrieval`\), job completion status, SNS topic name, job status code, and the vault ARN\. The following is an example notification S3 Glacier sent to an SNS topic after an `InventoryRetrieval` job completed\. 

```
{
 "Action": "InventoryRetrieval",
 "ArchiveId": null,
 "ArchiveSizeInBytes": null,
 "Completed": true,
 "CompletionDate": "2012-06-12T22:20:40.790Z",
 "CreationDate": "2012-06-12T22:20:36.814Z",
 "InventorySizeInBytes":11693,
 "JobDescription": "my retrieval job",
 "JobId":"HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID",
 "SHA256TreeHash":null,
 "SNSTopic": "arn:aws:sns:us-west-2:012345678901:mytopic",
 "StatusCode":"Succeeded",
 "StatusMessage": "Succeeded",
 "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
}
```

If the `Completed` field is true, you must also check the `StatusCode` to check if the job completed successfully or failed\. 

Note that the Amazon SNS topic must allow the vault to publish a notification\. By default, only the SNS topic owner can publish a message to the topic\. However, if the SNS topic and the vault are owned by different AWS accounts, then you must configure the SNS topic to accept publications from the vault\. You can configure the SNS topic policy in the Amazon SNS console\.  

For more information about Amazon SNS, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/gsg/Welcome.html)\.