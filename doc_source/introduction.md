# What Is Amazon S3 Glacier?<a name="introduction"></a>

Welcome to the *Amazon S3 Glacier Developer Guide*\. Amazon Simple Storage Service Glacier, that is Amazon S3 Glacier \(Glacier\), is a storage service optimized for infrequently used data, or "cold data\."

Glacier is an extremely low\-cost storage service that provides durable storage with security features for data archiving and backup\. With Glacier, customers can store their data cost effectively for months, years, or even decades\. Glacier enables customers to offload the administrative burdens of operating and scaling storage to AWS, so they don't have to worry about capacity planning, hardware provisioning, data replication, hardware failure detection and recovery, or time\-consuming hardware migrations\. For more service highlights and pricing information, go to the [Glacier detail page](https://aws.amazon.com/glacier)\. 

**Topics**
+ [Are You a First\-Time Glacier User?](#are-you-a-firsttime-glacier-user)
+ [Amazon S3 Glacier Data Model](amazon-glacier-data-model.md)
+ [Supported Operations in Glacier](amazon-glacier-supported-operations.md)
+ [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)

## Are You a First\-Time Glacier User?<a name="are-you-a-firsttime-glacier-user"></a>

If you are a first\-time user of Glacier, we recommend that you begin by reading the following sections:
+ **What is Glacier—**The rest of this section describes the underlying data model, the operations it supports, and the AWS SDKs that you can use to interact with the service\. 
+ **Getting Started—**The [Getting Started with Amazon S3 Glacier](amazon-glacier-getting-started.md) section walks you through the process of creating a vault, uploading archives, creating jobs to download archives, retrieving the job output, and deleting archives\. 
**Important**  
Glacier provides a console, which you can use to create and delete vaults\. However, all other interactions with Glacier require that you use the AWS Command Line Interface \(AWS CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\. For more information about using Glacier with the AWS CLI, go to [AWS CLI Reference for Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

Beyond the getting started section, you'll probably want to learn more about Glacier operations\. The following sections provide detailed information about working with Glacier using the REST API and the AWS Software Development Kits \(SDKs\) for Java and Microsoft \.NET: 
+ [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)

  This section provides an overview of the AWS SDKs used in various code examples in this guide\. A review of this section will help when reading the following sections\. It includes an overview of the high\-level and the low\-level APIs that these SDKs offer, when to use them, and common steps for running the code examples provided in this guide\. 
+ [Working with Vaults in Amazon S3 Glacier](working-with-vaults.md)

  This section provides details of various vault operations such as creating a vault, retrieving vault metadata, using jobs to retrieve vault inventory, and configuring vault notifications\. In addition to using the Glacier console, you can use the AWS SDKs for various vault operations\. This section describes the API and provides working samples using the AWS SDK for Java and \.NET\.
+ [Working with Archives in Amazon S3 Glacier](working-with-archives.md)

  This section provides details of archive operations such as uploading an archive in a single request or using a multipart upload operation to upload large archives in parts\. The section also explains creating jobs to download archives asynchronously\. The section provides examples using the AWS SDK for Java and \.NET\.
+ [API Reference for Glacier](amazon-glacier-api.md)

  Glacier is a RESTful service\. This section describes the REST operations, including the syntax, and example requests and responses for all the operations\. Note that the AWS SDK libraries wrap this API, simplifying your programming tasks\. 

Amazon Simple Storage Service \(Amazon S3\) supports lifecycle configuration on an S3 bucket, which enables you to transition objects to the Amazon S3 GLACIER storage class for archival\. When you transition Amazon S3 objects to the GLACIER storage class, Amazon S3 internally uses Glacier for durable storage at lower cost\. Although the objects are stored in Glacier, they remain Amazon S3 objects that you manage in Amazon S3, and you cannot access them directly through Glacier\.

For more information about Amazon S3 lifecycle configuration and transitioning objects to the GLACIER storage class, see [Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) and [Transitioning Objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service Developer Guide*\.