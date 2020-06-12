# Step 3: Upload an Archive to a Vault in Amazon S3 Glacier<a name="getting-started-upload-archive"></a>

In this step, you'll upload a sample archive to the vault you created in the preceding step \(see [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\)\. Depending on the development platform you are using, click one of the links at the end of this section\.

**Important**  
Any archive operation, such as upload, download, or deletion, requires that you use the AWS Command Line Interface \(CLI\) or write code\. There is no console support for archive operations\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. To install the AWS CLI, see [AWS Command Line Interface](http://aws.amazon.com/cli/)\. For more information about using Amazon S3 Glacier \(S3 Glacier\) with the AWS CLI, see [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. For examples of using the AWS CLI to upload archives to S3 Glacier, see [Using S3 Glacier with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-using-glacier.html)\. 

An archive is any object, such as a photo, video, or document that you store in a vault\. It is a base unit of storage in S3 Glacier\. You can upload an archive in a single request\. For large archives, S3 Glacier provides a multipart upload API that enables you to upload an archive in parts\. In this getting started section, you upload a sample archive in a single request\. For this exercise, you specify a file that is smaller in size\. For larger files, multipart upload is suitable\. For more information, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

**Topics**
+ [Upload an Archive to a Vault in Amazon S3 Glacier Using the AWS SDK for Java](getting-started-upload-archive-java.md)
+ [Upload an Archive to a Vault in Amazon S3 Glacier Using the AWS SDK for \.NET](getting-started-upload-archive-dotnet.md)