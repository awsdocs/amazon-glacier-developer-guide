# Step 4: Download an Archive from a Vault in S3 Glacier<a name="getting-started-download-archive"></a>

In this step, you'll download the sample archive that you uploaded previously in [Step 3: Upload an Archive to a Vault in S3 Glacier](getting-started-upload-archive.md)\.

 

**Important**  
Amazon S3 Glacier does provide a console\. However, any archive operation, such as upload, download, or deletion, requires you to use the AWS Command Line Interface \(CLI\) or write code\. There is no console support for archive operations\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\.   
To install the AWS CLI, see [AWS Command Line Interface](http://aws.amazon.com/cli/)\. For more information about using S3 Glacier with the AWS CLI, see [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. For examples of using the AWS CLI to upload archives to S3 Glacier, see [Using S3 Glacier with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-using-glacier.html)\. 

In general, retrieving your data from S3 Glacier is a two\-step process: 

1. Initiate a retrieval job\.

1. After the job is completed, download the bytes of data\. 

To retrieve an archive from S3 Glacier, you first initiate a job\. After the job is completed, you download the data\. For more information about archive retrievals, see [Retrieving S3 Glacier Archives Using AWS Console](downloading-an-archive-two-steps.md)\.

The access time of your request depends on the retrieval option that you choose: Expedited, Standard, or Bulk retrievals\. For all but the largest archives \(250 MB\+\), archives accessed by using Expedited retrievals are typically made available within 1–5 minutes\. Archives retrieved by using Standard retrievals typically are available between 3–5 hours\. Bulk retrievals typically are available within 5–12 hours\. For more information about the various retrieval options, see the [S3 Glacier FAQ](http://aws.amazon.com/glacier/faqs/#Data-retrievals)\. For information about data retrieval charges, see the [S3 Glacier pricing page](https://aws.amazon.com/s3/glacier/pricing/)\.

The code examples shown in the following topics initiate the job, wait for it to be completed, and then download the archive's data\. 

**Topics**
+ [Download an Archive from a Vault in S3 Glacier by Using the AWS SDK for Java](getting-started-download-archive-java.md)
+ [Download an Archive from a Vault in S3 Glacier by Using the AWS SDK for \.NET](getting-started-download-archive-dotnet.md)