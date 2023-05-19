# Retrieving S3 Glacier Archives Using AWS Console<a name="downloading-an-archive-two-steps"></a>

Retrieving an archive from Amazon S3 Glacier is an asynchronous operation in which you first initiate a job, and then download the output after the job is completed\. To initiate an archive retrieval job, you use the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) REST API operation or the equivalent in the AWS CLI, or the AWS SDKs\.

**Topics**
+ [Archive Retrieval Options](#api-downloading-an-archive-two-steps-retrieval-options)
+ [Ranged Archive Retrievals](#downloading-an-archive-range)

Retrieving an archive from S3 Glacier is a two\-step process\.

**To retrieve an archive**

1. Initiate an archive retrieval job\.

   1. Get the ID of the archive that you want to retrieve\. You can get the archive ID from an inventory of the vault\. You can get the archive ID with the REST API, AWS CLI, or AWS SDKs\. For more information, see [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)\. 

   1. Initiate a job that requests S3 Glacier to prepare an entire archive or a portion of the archive for subsequent download by using the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) operation\. 

   When you initiate a job, S3 Glacier returns a job ID in the response and runs the job asynchronously\. \(You cannot download the job output until after the job is completed, as described in Step 2\.\)
**Important**  
For Standard retrievals only, a data retrieval policy can cause your `Initiate Job` request to fail with a `PolicyEnforcedException` exception\. For more information about data retrieval policies, see [S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\. For more information about the `PolicyEnforcedException` exception, see [Error Responses](api-error-responses.md)\.

   When required, you can restore large segments of the data stored in S3 Glacier\. For more information about restoring data from the S3 Glacier storage classes, see [ Storage Classes for Archiving Objects]( https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) in the *Amazon Simple Storage Service User Guide*\.

1. After the job is completed, download the bytes by using the [Get Job Output \(GET output\)](api-job-output-get.md) operation\. 

   You can download all bytes or specify a byte range to download only a portion of the job output\. For larger output, downloading the output in chunks helps in the event of a download failure, such as a network failure\. If you get job output in a single request and there is a network failure, you have to restart downloading the output from the beginning\. However, if you download the output in chunks, in the event of any failure, you need only restart the download of the smaller portion and not the entire output\. 

S3 Glacier must complete a job before you can get its output\. After completion, a job does not expire for at least 24 hours, which means that you can download the output within the 24\-hour period after the job is completed\. To determine if your job is complete, check its status by using one of the following options:
+ **Wait for a job\-completion notification** – You can specify an Amazon Simple Notification Service \(Amazon SNS\) topic to which S3 Glacier can post a notification after the job is completed\. S3 Glacier sends a notification only after it completes the job\.

  You can specify an Amazon SNS topic for a job when you initiate the job\. In addition to specifying an Amazon SNS topic in your job request, if your vault has notifications set for archive retrieval events, then S3 Glacier also publishes a notification to that SNS topic\. For more information, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\.
+ **Request job information explicitly** – You can also use the S3 Glacier `Describe Job` API operation \([Describe Job \(GET JobID\)](api-describe-job-get.md)\) to periodically poll for job information\. However, we recommend using Amazon SNS notifications\.

**Note**  
The information that you get by using an Amazon SNS notification is the same as what you get by calling the `Describe Job` API operation\. 

## Archive Retrieval Options<a name="api-downloading-an-archive-two-steps-retrieval-options"></a>

When initiating a job to retrieve an archive, you can specify one of the following retrieval options, based on your access time and cost requirements\. For information about retrieval pricing, see [Amazon S3 Glacier Pricing](https://aws.amazon.com/s3/glacier/pricing/)\.
+ **Expedited** – Expedited retrievals allow you to quickly access your data that's stored in the S3 Glacier Flexible Retrieval storage class or the S3 Intelligent\-Tiering Archive Access tier when occasional urgent requests for restoring archives are required\. For all but the largest archives \(more than 250 MB\), data accessed by using Expedited retrievals is typically made available within 1–5 minutes\. Provisioned capacity ensures that retrieval capacity for Expedited retrievals is available when you need it\. For more information, see [Provisioned Capacity](#api-downloading-an-archive-two-steps-retrieval-expedited-capacity)\. 
+ **Standard** – Standard retrievals allow you to access any of your archives within several hours\. Standard retrievals are typically completed within 3–5 hours\. This is the default option for retrieval requests that do not specify the retrieval option\.
+ **Bulk** – Bulk retrievals are the lowest\-cost S3 Glacier retrieval option, which you can use to retrieve large amounts, even petabytes, of data inexpensively in a day\. Bulk retrievals are typically completed within 5–12 hours\.

To make an Expedited, Standard, or Bulk retrieval, set the `Tier` parameter in the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) REST API request to the option that you want, or the equivalent in the AWS CLI or AWS SDKs\. If you have purchased provisioned capacity, then all Expedited retrievals are automatically served through your provisioned capacity\. 

### Provisioned Capacity<a name="api-downloading-an-archive-two-steps-retrieval-expedited-capacity"></a>

Provisioned capacity helps ensure that your retrieval capacity for Expedited retrievals is available when you need it\. Each unit of capacity provides that at least three Expedited retrievals can be performed every 5 minutes and provides up to 150 megabytes per second \(MBps\) of retrieval throughput\.

If your workload requires highly reliable and predictable access to a subset of your data in minutes, we recommend that you purchase provisioned retrieval capacity\. Without provisioned capacity, Expedited retrievals are typically accepted, except for rare situations of unusually high demand\. However, if you require access to Expedited retrievals under all circumstances, you must purchase provisioned retrieval capacity\. 

#### Purchasing Provisioned Capacity<a name="downloading-an-archive-purchase-provisioned-capacity"></a>

You can purchase provisioned capacity units by using the S3 Glacier console, the [Purchase Provisioned Capacity \(POST provisioned\-capacity\)](api-PurchaseProvisionedCapacity.md) REST API operation, the AWS SDKs, or the AWS CLI\. For provisioned capacity pricing information, see [Amazon S3 Glacier Pricing](https://aws.amazon.com/s3/glacier/pricing/)\. 

A provisioned capacity unit lasts for one month, starting at the date and time of purchase\.

If the start date is on the 31st day of a month, the expiration date is the last day of the next month\. For example, if the start date is August 31, the expiration date is September 30\. If the start date is January 31, the expiration date is February 28\.

**To purchase provisioned capacity by using the Amazon S3 Glacier console**

1.  Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/home](https://console.aws.amazon.com/glacier/home)\.

1. In the left navigation pane, choose **Data retrieval settings**\.

1. Under **Provisioned capacity units \(PCUs\)**, choose **Purchase PCU**\. The **Purchase PCU** dialog box appears\.

1. If you want to purchase provisioned capacity, enter **confirm** in the **To confirm purchase** box\.

1.  Choose **Purchase PCU**\. 

## Ranged Archive Retrievals<a name="downloading-an-archive-range"></a>

When you retrieve an archive from S3 Glacier, you can optionally specify a range, or portion, of the archive to retrieve\. The default is to retrieve the whole archive\. Specifying a range of bytes can be helpful when you want to do the following:
+ **Manage your data downloads** – S3 Glacier allows retrieved data to be downloaded for 24 hours after the retrieval request is completed\. Therefore, you might want to retrieve only portions of the archive so that you can manage the schedule of downloads within the given download window\.
+ **Retrieve a targeted part of a large archive** – For example, suppose you have previously aggregated many files and uploaded them as a single archive, and now you want to retrieve a few of the files\. In this case, you can specify a range of the archive that contains the files that you are interested in by using one retrieval request\. Or, you can initiate multiple retrieval requests, each with a range for one or more files\.

When initiating a retrieval job using range retrievals, you must provide a range that is megabyte aligned\. In other words, the byte range can start at zero \(the beginning of your archive\), or at any 1\-MB interval thereafter \(1 MB, 2 MB, 3 MB, and so on\)\. 

The end of the range can either be the end of your archive or any 1 MB interval greater than the beginning of your range\. Furthermore, if you want to get checksum values when you download the data \(after the retrieval job is completed\), the range that you request in the job initiation must also be tree\-hash aligned\. You can use checksums to help ensure that your data was not corrupted during transmission\. For more information about megabyte alignment and tree\-hash alignment, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\. 