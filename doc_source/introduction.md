# What Is Amazon S3 Glacier?<a name="introduction"></a>

Amazon S3 Glacier \(S3 Glacier\) is a secure and durable service for low\-cost data archiving and long\-term backup\.

With S3 Glacier, you can store your data cost effectively for months, years, or even decades\. S3 Glacier helps you offload the administrative burdens of operating and scaling storage to AWS, so you don't have to worry about capacity planning, hardware provisioning, data replication, hardware failure detection and recovery, or time\-consuming hardware migrations\. For more information, go to the [S3 Glacier pricing page](https://aws.amazon.com/s3/glacier/pricing/)\.

Amazon Simple Storage Service \(Amazon S3\) also provides three Amazon S3 Glacier archive storage classes\. These storage classes are designed for different access patterns and storage duration\. These storage classes differ as follows:
+ **S3 Glacier Instant Retrieval** – Use for archiving data that is rarely accessed and requires milliseconds retrieval\. 
+ **S3 Glacier Flexible Retrieval** \(formerly the S3 Glacier storage class\) – Use for archives where portions of the data might need to be retrieved in minutes\. Data stored in the S3 Glacier Flexible Retrieval storage class can be accessed in as little as 1\-5 minutes by using Expedited retrieval\. You can also request free Bulk retrievals in up to 5\-12 hours\.
+ **S3 Glacier Deep Archive** – Use for archiving data that rarely needs to be accessed\. Data stored in the S3 Glacier Deep Archive storage class has a default retrieval time of 12 hours\. 

**Important**  
S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive objects are not available for real\-time access\. You must first restore S3 Glacier Flexible Retrieval and S3 Glacier Deep Archive objects before you can access them\. For these storage classes, Amazon S3 supports restore requests at a rate of up to 1,000 transactions per second, per AWS account\.  
When you choose the S3 Glacier Flexible Retrieval or S3 Glacier Deep Archive storage class, your objects remain in Amazon S3\. You can't access them directly through the Amazon S3 Glacier service\.

For more information about the Amazon S3 Glacier storage classes, see [ Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-infreq-data-access) in the *Amazon Simple Storage Service User Guide*\. 

For an enhanced S3 Glacier experience, you can use the Amazon S3 console instead of the Amazon S3 Glacier service\. Accessing and using the Amazon S3 Glacier storage classes through the Amazon S3 APIs and Amazon S3 console provides enhanced functionality for data management and cost optimization\. 

Amazon S3 supports lifecycle configuration on an S3 bucket, which enables you to transition objects to the S3 Glacier storage classes for archival\. For more information about Amazon S3 lifecycle configuration and transitioning objects to the S3 Glacier storage classes, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) and [Transitioning Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service User Guide*\.

The Amazon S3 Glacier storage classes are only some of the different storage classes for Amazon S3\. For a general overview of Amazon S3 core concepts, such as buckets, access points, storage classes and objects, see [What is Amazon S3?](https://docs.aws.amazon.com/AmazonS3/latest/dev/introduction.html) in the *Amazon Simple Storage Service User Guide*\. 

**Topics**
+ [Are You a First\-Time S3 Glacier User?](#are-you-a-firsttime-glacier-user)
+ [Amazon S3 Glacier Data Model](amazon-glacier-data-model.md)
+ [Supported Operations in S3 Glacier](amazon-glacier-supported-operations.md)
+ [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)

## Are You a First\-Time S3 Glacier User?<a name="are-you-a-firsttime-glacier-user"></a>

If you are a first\-time user of S3 Glacier, we recommend that you begin by reading the following sections:

 
+ **What is Amazon S3 Glacier** – The rest of this section describes the underlying data model, the operations it supports, and the AWS SDKs that you can use to interact with the service\. 
+ **Getting Started** – The [Getting Started with Amazon S3 Glacier](amazon-glacier-getting-started.md) section walks you through the process of creating a vault, uploading archives, creating jobs to download archives, retrieving the job output, and deleting archives\. 
**Important**  
S3 Glacier does provide a console\. However, any archive operation, such as upload, download, or deletion, requires you to use the AWS Command Line Interface \(AWS CLI\) or write code\. There is no console support for archive operations\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\.   
To install the AWS CLI, see [AWS Command Line Interface](http://aws.amazon.com/cli/)\. For more information about using S3 Glacier with the AWS CLI, see the [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. For examples of using the AWS CLI to upload archives to S3 Glacier, see [Using S3 Glacier with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-using-glacier.html)\. 

Beyond the getting started section, you'll probably want to learn more about S3 Glacier operations\. The following sections provide detailed information about working with S3 Glacier by using the REST API and the AWS SDKs for Java and Microsoft \.NET: 
+ [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)

  This section provides an overview of the AWS SDKs used in various code examples in this guide\. A review of this section will help when reading the following sections\. It includes an overview of the high\-level and the low\-level APIs that these SDKs offer, when to use them, and common steps for running the code examples provided in this guide\. 
+ [Working with Vaults in Amazon S3 Glacier](working-with-vaults.md)

  This section provides details of various vault operations, such as creating a vault, retrieving vault metadata, using jobs to retrieve vault inventory, and configuring vault notifications\. In addition to using the S3 Glacier console, you can use the AWS SDKs for various vault operations\. This section describes the API and provides working samples by using the AWS SDK for Java and the AWS SDK for \.NET\.
+ [Working with Archives in Amazon S3 Glacier](working-with-archives.md)

  This section provides details of archive operations, such as uploading an archive in a single request or using a multipart upload operation to upload large archives in parts\. The section also explains how to create jobs to download archives asynchronously\. The section provides examples by using the AWS SDK for Java and the AWS SDK for \.NET\.
+ [API Reference for Amazon S3 Glacier](amazon-glacier-api.md)

  S3 Glacier is a RESTful service\. This section describes the REST operations, including the syntax, and example requests and responses for all the operations\. The AWS SDK libraries wrap this API, simplifying your programming tasks\. 