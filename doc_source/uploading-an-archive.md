# Uploading an Archive in Amazon Glacier<a name="uploading-an-archive"></a>

Amazon Glacier provides a management console, which you can use to create and delete vaults\. However, you cannot upload archives to Amazon Glacier by using the management console\. To upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\. 

For information about using Amazon Glacier with the AWS CLI, go to [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\. The following **Uploading** topics describe how to upload archives to Amazon Glacier by using the AWS SDK for Java, the AWS SDK for \.NET, and the REST API\.

**Topics**
+ [Options for Uploading an Archive to Amazon Glacier](#uploading-an-archive-overview)
+ [Uploading an Archive in a Single Operation](uploading-archive-single-operation.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)

## Options for Uploading an Archive to Amazon Glacier<a name="uploading-an-archive-overview"></a>

Depending on the size of the data you are uploading, Amazon Glacier offers the following options: 
+ **Upload archives in a single operation** – In a single operation, you can upload archives from 1 byte to up to 4 GB in size\. However, we encourage Amazon Glacier customers to use multipart upload to upload archives greater than 100 MB\. For more information, see [Uploading an Archive in a Single Operation](uploading-archive-single-operation.md)\.
+ **Upload archives in parts** – Using the multipart upload API, you can upload large archives, up to about 40,000 GB \(10,000 \* 4 GB\)\. 

  The multipart upload API call is designed to improve the upload experience for larger archives\. You can upload archives in parts\. These parts can be uploaded independently, in any order, and in parallel\. If a part upload fails, you only need to upload that part again and not the entire archive\. You can use multipart upload for archives from 1 byte to about 40,000 GB in size\. For more information, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

**Important**  
The Amazon Glacier vault inventory is only updated once a day\. When you upload an archive, you will not immediately see the new archive added to your vault \(in the console or in your downloaded vault inventory list\) until the vault inventory has been updated\.

### Using the AWS Snowball Service<a name="using-import-export-service-for-glacier"></a>

AWS Snowball accelerates moving large amounts of data into and out of AWS using Amazon\-owned devices, bypassing the internet\. For more information, see [AWS Snowball](http://aws.amazon.com/snowball) detail page\. 

To upload existing data to Amazon Glacier, you might consider using one of the AWS Snowball device types to import data into Amazon Simple Storage Service \(Amazon S3\), and then move it  to the Amazon S3 GLACIER storage class for archival using lifecycle rules\. When you transition Amazon S3 objects to the GLACIER storage class, Amazon S3 internally uses Amazon Glacier for durable storage at lower cost\. Although the objects are stored in Amazon Glacier, they remain Amazon S3 objects that you manage in Amazon S3, and you cannot access them directly through Amazon Glacier\.

For more information about Amazon S3 lifecycle configuration and transitioning objects to the GLACIER storage class, see [Object Lifecycle Management](http://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html) and [Transitioning Objects](http://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service Developer Guide*\.