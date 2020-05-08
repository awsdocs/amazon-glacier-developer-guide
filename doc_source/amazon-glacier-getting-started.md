# Getting Started with Amazon S3 Glacier<a name="amazon-glacier-getting-started"></a>

In Amazon S3 Glacier \(S3 Glacier\), a vault is a container for storing archives, and an archive is any object, such as a photo, video, or document that you store in a vault\. An archive is the base unit of storage in S3 Glacier\. This getting started exercise provides instructions for you to explore basic S3 Glacier operations on the vaults and archives resources described in the [Amazon S3 Glacier Data Model](amazon-glacier-data-model.md) section\. 

In the getting started exercise, you will create a vault, upload and download an archive, and finally delete the archive and the vault\. You can do all these operations programmatically\. However, the getting started exercise uses the S3 Glacier management console to create and delete a vault\. For uploading and downloading an archive, this getting started section uses the AWS Software Development Kits \(SDKs\) for Java and \.NET high\-level API\. The high\-level API provides a simplified programming experience when working with S3 Glacier\. For more information about these APIs, see [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)\.

**Important**  
S3 Glacier provides a management console\. You can use the console to create and delete vaults as shown in this getting started exercise\. However, all other interactions with S3 Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using S3 Glacier with the AWS CLI, go to [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

This getting started exercise provides code examples in Java and C\# for you to upload and download an archive\. The last section of the getting started provides steps where you can learn more about the developer experience with S3 Glacier\.

**Topics**
+ [Step 1: Before You Begin with Amazon S3 Glacier](getting-started-before-you-begin.md)
+ [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)
+ [Step 3: Upload an Archive to a Vault in Amazon S3 Glacier](getting-started-upload-archive.md)
+ [Step 4: Download an Archive from a Vault in Amazon S3 Glacier](getting-started-download-archive.md)
+ [Step 5: Delete an Archive from a Vault in Amazon S3 Glacier](getting-started-delete-archive.md)
+ [Step 6: Delete a Vault in Amazon S3 Glacier](getting-started-delete-vault.md)
+ [Where Do I Go From Here?](getting-started-where-do-i-go-next.md)