# What Is Amazon S3 Glacier?<a name="introduction"></a>

Amazon S3 Glacier is a secure, durable, and extremely low\-cost Amazon S3 storage class for data archiving and long\-term backup\.

With S3 Glacier, customers can store their data cost effectively for months, years, or even decades\. S3 Glacier enables customers to offload the administrative burdens of operating and scaling storage to AWS, so they don't have to worry about capacity planning, hardware provisioning, data replication, hardware failure detection and recovery, or time\-consuming hardware migrations\. For more service highlights and pricing information, go to the [S3 Glacier detail page](https://aws.amazon.com/glacier)\.

S3 Glacier is one of the many different storage classes for Amazon S3\. For a general overview of Amazon S3 core concepts, such as buckets, access points, storage classes and objects, see [What is Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/introduction.html) in the *Amazon Simple Storage Service Developer Guide*\. 

**Topics**
+ [Are You a First\-Time S3 Glacier User?](#are-you-a-firsttime-glacier-user)
+ [Amazon S3 Glacier Data Model](amazon-glacier-data-model.md)
+ [Supported Operations in S3 Glacier](amazon-glacier-supported-operations.md)
+ [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)

## Are You a First\-Time S3 Glacier User?<a name="are-you-a-firsttime-glacier-user"></a>

If you are a first\-time user of S3 Glacier, we recommend that you begin by reading the following sections:
+ **What is Amazon S3 Glacier—**The rest of this section describes the underlying data model, the operations it supports, and the AWS SDKs that you can use to interact with the service\. 
+ **Getting Started—**The [Getting Started with Amazon S3 Glacier](amazon-glacier-getting-started.md) section walks you through the process of creating a vault, uploading archives, creating jobs to download archives, retrieving the job output, and deleting archives\. 
**Important**  
S3 Glacier provides a console, which you can use to create and delete vaults\. However, all other interactions with S3 Glacier require that you use the AWS Command Line Interface \(AWS CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\. For more information about using S3 Glacier with the AWS CLI, go to [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

Beyond the getting started section, you'll probably want to learn more about S3 Glacier operations\. The following sections provide detailed information about working with S3 Glacier using the REST API and the AWS Software Development Kits \(SDKs\) for Java and Microsoft \.NET: 
+ [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)

  This section provides an overview of the AWS SDKs used in various code examples in this guide\. A review of this section will help when reading the following sections\. It includes an overview of the high\-level and the low\-level APIs that these SDKs offer, when to use them, and common steps for running the code examples provided in this guide\. 
+ [Working with Vaults in Amazon S3 Glacier](working-with-vaults.md)

  This section provides details of various vault operations such as creating a vault, retrieving vault metadata, using jobs to retrieve vault inventory, and configuring vault notifications\. In addition to using the S3 Glacier console, you can use the AWS SDKs for various vault operations\. This section describes the API and provides working samples using the AWS SDK for Java and \.NET\.
+ [Working with Archives in Amazon S3 Glacier](working-with-archives.md)

  This section provides details of archive operations such as uploading an archive in a single request or using a multipart upload operation to upload large archives in parts\. The section also explains creating jobs to download archives asynchronously\. The section provides examples using the AWS SDK for Java and \.NET\.
+ [API Reference for Amazon S3 Glacier](amazon-glacier-api.md)

  S3 Glacier is a RESTful service\. This section describes the REST operations, including the syntax, and example requests and responses for all the operations\. Note that the AWS SDK libraries wrap this API, simplifying your programming tasks\. 

Amazon Simple Storage Service \(Amazon S3\) supports lifecycle configuration on an S3 bucket, which enables you to transition objects to the S3 Glacier storage class for archival\. When you transition Amazon S3 objects to the S3 Glacier storage class, Amazon S3 internally uses S3 Glacier for durable storage at lower cost\. Although the objects are stored in S3 Glacier, they remain Amazon S3 objects that you manage in Amazon S3, and you cannot access them directly through S3 Glacier\.

For more information about Amazon S3 lifecycle configuration and transitioning objects to the S3 Glacier storage class, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) and [Transitioning Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service Developer Guide*\.