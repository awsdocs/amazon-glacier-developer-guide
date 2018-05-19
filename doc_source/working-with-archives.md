# Working with Archives in Amazon Glacier<a name="working-with-archives"></a>

An archive is any object, such as a photo, video, or document, that you store in a vault\. It is a base unit of storage in Amazon Glacier\. Each archive has a unique ID and an optional description\. When you upload an archive, Amazon Glacier returns a response that includes an archive ID\. This archive ID is unique in the region in which the archive is stored\. The following is an example archive ID\. 

```
TJgHcrOSfAkV6hdPqOATYfp_0ZaxL1pIBOc02iZ0gDPMr2ig-nhwd_PafstsdIf6HSrjHnP-3p6LCJClYytFT_CBhT9CwNxbRaM5MetS3I-GqwxI3Y8QtgbJbhEQPs0mJ3KExample
```

 Archive IDs are 138 bytes long\. When you upload an archive, you can provide an optional description\. You can retrieve an archive using its ID but not its description\.

**Important**  
Amazon Glacier provides a management console\. You can use the console to create and delete vaults\. However, all other interactions with Amazon Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using Amazon Glacier with the AWS CLI, go to [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

**Topics**
+ [Archive Operations in Amazon Glacier](#archive-operations-quick-intro)
+ [Maintaining Client\-Side Archive Metadata](#client-side-key-map-concept)
+ [Uploading an Archive in Amazon Glacier](uploading-an-archive.md)
+ [Downloading an Archive in Amazon Glacier](downloading-an-archive.md)
+ [Deleting an Archive in Amazon Glacier](deleting-an-archive.md)
+ [Querying an Archives in Amazon Glacier](querying-archives.md)

## Archive Operations in Amazon Glacier<a name="archive-operations-quick-intro"></a>

Amazon Glacier supports the following basic archive operations: upload, download, and delete\. Downloading an archive is an asynchronous operation\. 

### Uploading an Archive in Amazon Glacier<a name="uploading-an-archive-quick-intro"></a>

You can upload an archive in a single operation or upload it in parts\. The API call you use to upload an archive in parts is referred as Multipart Upload\. For more information, see [Uploading an Archive in Amazon Glacier](uploading-an-archive.md)\.

**Important**  
Amazon Glacier provides a management console\. You can use the console to create and delete vaults\. However, all other interactions with Amazon Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using Amazon Glacier with the AWS CLI, go to [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

### Downloading an Archive in Amazon Glacier<a name="downloading-an-archive-quick-intro"></a>

Downloading an archive is an asynchronous operation\. You must first initiate a job to download a specific archive\. After receiving the job request, Amazon Glacier prepares your archive for download\. After the job completes, you can download your archive data\. Because of the asynchronous nature of the job, you can request Amazon Glacier to send a notification to an Amazon Simple Notification Service \(Amazon SNS\) topic when the job completes\. You can specify an SNS topic for each individual job request or configure your vault to send a notification when specific events occur\. For more information about downloading an archive, see [Downloading an Archive in Amazon Glacier](downloading-an-archive.md)\.

### Deleting an Archive in Amazon Glacier<a name="deleting-an-archive-quick-intro"></a>

Amazon Glacier provides an API call that you can use to delete one archive at a time\. For more information, see [Deleting an Archive in Amazon Glacier](deleting-an-archive.md)\.

### Updating an Archive in Amazon Glacier<a name="updating-an-archive-quick-intro"></a>

After you upload an archive, you cannot update its content or its description\. The only way you can update the archive content or its description is by deleting the archive and uploading another archive\. Note that each time you upload an archive, Amazon Glacier returns to you a unique archive ID\.

## Maintaining Client\-Side Archive Metadata<a name="client-side-key-map-concept"></a>

Except for the optional archive description, Amazon Glacier does not support any additional metadata for the archives\. When you upload an archive Amazon Glacier assigns an ID, an opaque sequence of characters, from which you cannot infer any meaning about the archive\. You might maintain metadata about the archives on the client\-side\. The metadata can include archive name and some other meaningful information about the archive\. 

**Note**  
If you are an Amazon Simple Storage Service \(Amazon S3\) customer, you know that when you upload an object to a bucket, you can assign the object an object key such as `MyDocument.txt` or `SomePhoto.jpg`\. In Amazon Glacier, you cannot assign a key name to the archives you upload\. 

If you maintain client\-side archive metadata, note that Amazon Glacier maintains a vault inventory that includes archive IDs and any descriptions you provided during the archive upload\. You might occasionally download the vault inventory to reconcile any issues in your client\-side database you maintain for the archive metadata\. However, Amazon Glacier takes vault inventory approximately daily\. When you request a vault inventory, Amazon Glacier returns the last inventory it prepared, a point in time snapshot\.