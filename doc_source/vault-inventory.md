# Downloading a Vault Inventory in Amazon Glacier<a name="vault-inventory"></a>

After you upload your first archive to your vault, Amazon Glacier automatically creates a vault inventory and then updates it approximately once a day\. After Amazon Glacier creates the first inventory, it typically takes half a day and up to a day before that inventory is available for retrieval\. You can retrieve a vault inventory from Amazon Glacier with the following two\-step process: 

1. Initiate a retrieval job\. 
**Important**  
A data retrieval policy can cause your initiate retrieval job request to fail with a `PolicyEnforcedException` exception\. For more information about data retrieval policies, see [Amazon Glacier Data Retrieval Policies](data-retrieval-policy.md)\. For more information about the `PolicyEnforcedException` exception, see [Error Responses](api-error-responses.md)\.

1. After the job completes, download the bytes\. 

For example, retrieving an archive or a vault inventory requires you to first initiate a retrieval job\. The job request is executed asynchronously\. When you initiate a retrieval job, Amazon Glacier creates a job and returns a job ID in the response\. When Amazon Glacier completes the job, you can get the job output, the archive bytes, or the vault inventory data\. 

The job must complete before you can get its output\. To determine the status of the job, you have the following options:
+ **Wait for job completion notification**—You can specify an Amazon Simple Notification Service \(Amazon SNS\) topic to which Amazon Glacier can post a notification after the job is completed\. You can specify Amazon SNS topic using the following methods: 
  + Specify an Amazon SNS topic per job basis\. 

    When you initiate a job, you can optionally specify an Amazon SNS topic\.
  + Set notification configuration on the vault\.

    You can set notification configuration for specific events on the vault \(see [Configuring Vault Notifications in Amazon Glacier](configuring-notifications.md)\)\. Amazon Glacier sends a message to the specified SNS topic any time the specific event occur\.

  If you have notification configuration set on the vault and you also specify an Amazon SNS topic when you initiate a job, Amazon Glacier sends job completion message to both the topics\. 

  You can configure the SNS topic to notify you via email or store the message in an Amazon Simple Queue Service \(Amazon SQS\) that your application can poll\. When a message appears in the queue, you can check if the job is completed successfully and then download the job output\. 
+ **Request job information explicitly**—Amazon Glacier also provides a describe job operation \([Describe Job \(GET JobID\)](api-describe-job-get.md)\) that enables you to poll for job information\. You can periodically send this request to obtain job information\. However, using Amazon SNS notifications is the recommended option\.

**Note**  
The information you get via SNS notification is the same as what you get by calling Describe Job\. 

**Topics**
+ [About the Inventory](#vault-inventory-about)
+ [Downloading a Vault Inventory in Amazon Glacier Using the AWS SDK for Java](retrieving-vault-inventory-java.md)
+ [Downloading a Vault Inventory in Amazon Glacier Using the AWS SDK for \.NET](retrieving-vault-inventory-sdk-dotnet.md)
+ [Downloading a Vault Inventory Using the REST API](retrieving-vault-inventory-rest-api.md)

## About the Inventory<a name="vault-inventory-about"></a>

Amazon Glacier updates a vault inventory approximately once a day, starting on the day you first upload an archive to the vault\. If there have been no archive additions or deletions to the vault since the last inventory, the inventory date is not updated\. When you initiate a job for a vault inventory, Amazon Glacier returns the last inventory it generated, which is a point\-in\-time snapshot and not real\-time data\. Note that after Amazon Glacier creates the first inventory for the vault, it typically takes half a day and up to a day before that inventory is available for retrieval\.

 You might not find it useful to retrieve a vault inventory for each archive upload\. However, suppose you maintain a database on the client\-side associating metadata about the archives you upload to Amazon Glacier\. Then, you might find the vault inventory useful to reconcile information, as needed, in your database with the actual vault inventory\. You can limit the number of inventory items retrieved by filtering on the archive creation date or by setting a limit\. For more information about limiting inventory retrieval, see [Range Inventory Retrieval](api-initiate-job-post.md#api-initiate-job-post-vault-inventory-list-filtering)\.

The inventory can be returned in two formats, comma\-separated values \(CSV\) or JSON\. You can optionally specify the format when you initiate the inventory job\. The default format is JSON\. For more information about the data fields returned in an inventory job output, see [Response Body](api-job-output-get.md#api-job-output-get-responses-elements) of the *Get Job Output API*\.