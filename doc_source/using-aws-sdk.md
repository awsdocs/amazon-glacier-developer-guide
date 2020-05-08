# Using the AWS SDKs with Amazon S3 Glacier<a name="using-aws-sdk"></a>

Amazon Web Services provides SDKs for you to develop applications for Amazon S3 Glacier \(S3 Glacier\)\. The SDK libraries wrap the underlying S3 Glacier API, simplifying your programming tasks\. For example, for each request sent to S3 Glacier, you must include a signature to authenticate your requests\. When you use the SDK libraries, you need to provide only your AWS security credentials in your code and the libraries compute the necessary signature and include it in the request sent to S3 Glacier\. The AWS SDKs provide libraries that map to the underlying REST API and provide objects that you can use to easily construct requests and process responses\. 

**Topics**
+ [AWS SDKs that Support S3 Glacier](#using-aws-sdk-with-glacier)
+ [AWS SDK Libraries for Java and \.NET](#java-.net-sdk-libraries)
+ [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)
+ [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)

## AWS SDKs that Support S3 Glacier<a name="using-aws-sdk-with-glacier"></a>

S3 Glacier is supported by the following AWS SDKs: 
+ [AWS SDK for C\+\+](https://aws.amazon.com/sdk-for-cpp/) 
+  [AWS SDK for Go](https://aws.amazon.com/sdk-for-go/)
+ [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/) 
+ [AWS SDK for JavaScript in Node\.js](https://aws.amazon.com/sdk-for-node-js/) 
+ [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/) 
+ [AWS SDK for PHP](https://aws.amazon.com/sdk-for-php/) 
+ [AWS SDK for Python \(Boto\)](https://aws.amazon.com/sdk-for-python/) 
+ [AWS SDK for Ruby](https://aws.amazon.com/sdk-for-ruby/) 

You can find examples of working with S3 Glacier using the Java and \.NET SDKs throughout this developer guide\. For libraries and sample code in all languages, see [Sample Code & Libraries](https://aws.amazon.com/code/)\. 

The AWS Command Line Interface \(AWS CLI\) is a unified tool to manage your AWS services, including S3 Glacier\. For information about downloading the AWS CLI, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\. For a list of the S3 Glacier CLI commands, see [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. 

## AWS SDK Libraries for Java and \.NET<a name="java-.net-sdk-libraries"></a>

The AWS SDKs for Java and \.NET offer high\-level and low\-level wrapper libraries\. 

### What Is the Low\-Level API?<a name="what-is-low-level-api"></a>

The low\-level wrapper libraries map closely the underlying REST API \([API Reference for Amazon S3 Glacier](amazon-glacier-api.md)\) supported by S3 Glacier\. For each S3 Glacier REST operations, the low\-level API provides a corresponding method, a request object for you to provide request information and a response object for you to process S3 Glacier response\. The low\-level wrapper libraries are the most complete implementation of the underlying S3 Glacier operations\. 

For information about these SDK libraries, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md) and [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\.

### What Is the High\-Level API?<a name="what-is-high-level-api"></a>

To further simplify application development, these libraries offer a higher\-level abstraction for some of the operations\. For example, 
+ Uploading an archive—To upload an archive using the low\-level API in addition to the file name and the vault name where you want to save the archive, You need to provide a checksum \(SHA\-256 tree hash\) of the payload\. However, the high\-level API computes the checksum for you\.
+ Downloading an archive or vault inventory—To download an archive using the low\-level API you first initiate a job, wait for the job to complete, and then get the job output\. You need to write additional code to set up an Amazon Simple Notification Service \(Amazon SNS\) topic for S3 Glacier to notify you when the job is complete\. You also need some polling mechanism to check if a job completion message was posted to the topic\. The high\-level API provides a method to download an archive that takes care of all these steps\. You only specify an archive ID and a folder path where you want to save the downloaded data\. 

For information about these SDK libraries, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md) and [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\.

### When to Use the High\-Level and Low\-Level API<a name="when-to-use-high-low-api"></a>

In general, if the high\-level API provides methods you need to perform an operation, you should use the high\-level API because of the simplicity it provides\. However, if the high\-level API does not offer the functionality, you can use the low\-level API\. Additionally, the low\-level API allows granular control of the operation such as retry logic in the event of a failure\. For example, when uploading an archive the high\-level API uses the file size to determine whether to upload the archive in a single operation or use the multipart upload API\. The API also has built\-in retry logic in case an upload fails\. However, your application might need granular control over these decisions, in which case you can use the low\-level API\.