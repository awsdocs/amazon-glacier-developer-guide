# Uploading an Archive in Amazon Glacier<a name="uploading-an-archive"></a>

Amazon Glacier provides a management console, which you can use to create and delete vaults\. However, you cannot upload archives to Amazon Glacier by using the management console\. To upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\. 

For information about using Amazon Glacier with the AWS CLI, go to [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\. The following **Uploading** topics describe how to upload archives to Amazon Glacier by using the AWS SDK for Java, the AWS SDK for \.NET, and the REST API\.


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

### Using the AWS Import/Export Service<a name="using-import-export-service-for-glacier"></a>

To upload existing data to Amazon Glacier, you might consider using the AWS Import/Export service\. AWS Import/Export accelerates moving large amounts of data into and out of AWS using portable storage devices for transport\. AWS transfers your data directly onto and off of storage devices using Amazon's high\-speed internal network, bypassing the Internet\. For more information, go to the [AWS Import/Export](http://aws.amazon.com/importexport) detail page\. 