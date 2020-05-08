# Using the AWS SDK for Java with Amazon S3 Glacier<a name="using-aws-sdk-for-java"></a>

The AWS SDK for Java provides both high\-level and low\-leve APIs for Amazon S3 Glacier \(S3 Glacier\) as described in [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)\. For information about downloading the AWS SDK for Java, see [AWS SDK for Java](http://aws.amazon.com/sdkforjava/)\.

**Note**  
The AWS SDK for Java provides thread\-safe clients for accessing S3 Glacier\. As a best practice, your applications should create one client and reuse the client between threads\.

**Topics**
+ [Using the Low\-Level API](#about-low-level-java-api)
+ [Using the High\-Level API](#about-high-level-java-api)
+ [Running Java Examples for Amazon S3 Glacier Using Eclipse](#setting-up-and-testing-sdk-java)
+ [Setting the Endpoint](#setting-sdk-java-endpoint)

## Using the Low\-Level API<a name="about-low-level-java-api"></a>

The low\-level `AmazonGlacierClient` class provides all the methods that map to the underlying REST operations of S3 Glacier \( [API Reference for Amazon S3 Glacier](amazon-glacier-api.md)\)\. When calling any of these methods, you must create a corresponding request object and provide a response object in which the method can return the S3 Glacier response to the operation\. 

For example, the `AmazonGlacierClient` class provides the `createVault` method to create a vault\. This method maps to the underlying Create Vault REST operation \(see [Create Vault \(PUT vault\)](api-vault-put.md)\)\. To use this method, you must create instances of the `CreateVaultResult` object that receives the S3 Glacier response as shown in the following Java code snippet:

```
AmazonGlacierClient client = new AmazonGlacierClient(credentials);
client.setEndpoint("https://glacier.us-west-2.amazonaws.com/");

CreateVaultRequest request = new CreateVaultRequest()
    .withAccountId("-")
    .withVaultName(vaultName);
CreateVaultResult result = client.createVault(createVaultRequest);
```

All the low\-level samples in the guide use this pattern\. 

**Note**  
The preceding code segment specifies `AccountID` when creating the request\. However, when using the AWS SDK for Java, the `AccountId` in the request is optional, and therefore all the low\-level examples in this guide don't set this value\. The `AccountId` is the AWS Account ID\. This value must match the AWS Account ID associated with the credentials used to sign the request\. You can specify either the AWS Account ID or optionally a '\-', in which case S3 Glacier uses the AWS Account ID associated with the credentials used to sign the request\. If you specify your Account ID, do not include hyphens in it\. When using AWS SDK for Java, if you don't provide the account ID, the library sets the account ID to '\-'\. 

## Using the High\-Level API<a name="about-high-level-java-api"></a>

To further simplify your application development, the AWS SDK for Java provides the `ArchiveTransferManager` class that implements a higher\-level abstraction for the some of the methods in the low\-level API\. It provides useful methods, such as the `upload` and `download` methods for archive operations\. 

For example, the following Java code snippet uses the `upload` high\-level method to upload an archive\. 

```
String vaultName = "examplevault";
String archiveToUpload = "c:/folder/exampleArchive.zip";

ArchiveTransferManager atm = new ArchiveTransferManager(client, credentials);
String archiveId = atm.upload(vaultName, "Tax 2012 documents", new File(archiveToUpload)).getArchiveId();
```

Note that any operations you perform apply to the AWS Region you specified when creating the `ArchiveTransferManager` object\. If you don't specify any AWS Region, the AWS SDK for Java sets `us-east-1` as the default AWS Region\.

All the high\-level examples in this guide use this pattern\. 

**Note**  
The high\-level `ArchiveTransferManager` class can be constructed with an `AmazonGlacierClient` instance or an `AWSCredentials` instance\.

## Running Java Examples for Amazon S3 Glacier Using Eclipse<a name="setting-up-and-testing-sdk-java"></a>

The easiest way to get started with the Java code examples is to install the latest AWS Toolkit for Eclipse\. For information on installing or updating to the latest toolkit, go to [http://aws\.amazon\.com/eclipse](http://aws.amazon.com/eclipse)\. The following tasks guide you through the creation and testing of the Java code examples provided in this section\.


**General Process of Creating Java Code Examples**  

|  |  | 
| --- |--- |
| 1 |  Create a default credentials profile for your AWS credentials as described in the AWS SDK for Java topic [Providing AWS Credentials in the AWS SDK for Java](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/credentials.html)\.   | 
| 2 | Create a new AWS Java project in Eclipse\. The project is pre\-configured with the AWS SDK for Java\. | 
| 3 | Copy the code from the section you are reading to your project\.  | 
| 4 | Update the code by providing any required data\. For example, if uploading a file, provide the file path and the bucket name\. | 
| 5 | Run the code\. Verify that the object is created by using the AWS Management Console\. For more information about the AWS Management Console, go to [http://aws\.amazon\.com/console/](http://aws.amazon.com/console/)\. | 

## Setting the Endpoint<a name="setting-sdk-java-endpoint"></a>

By default, the AWS SDK for Java uses the endpoint `https://glacier.us-east-1.amazonaws.com`\. You can set the endpoint explicitly as shown in the following Java code snippets\.

The following snippet shows how to set the endpoint to the US West \(Oregon\) Region \(`us-west-2`\) in the low\-level API\.

**Example**  

```
client = new AmazonGlacierClient(credentials);
client.setEndpoint("glacier.us-west-2.amazonaws.com");
```

The following snippet shows how to set the endpoint to the US West \(Oregon\) Region in the high\-level API\.

```
glacierClient = new AmazonGlacierClient(credentials);
sqsClient = new AmazonSQSClient(credentials);
snsClient = new AmazonSNSClient(credentials);
        
glacierClient.setEndpoint("glacier.us-west-2.amazonaws.com");
sqsClient.setEndpoint("sqs.us-west-2.amazonaws.com");
snsClient.setEndpoint("sns.us-west-2.amazonaws.com");

ArchiveTransferManager atm = new ArchiveTransferManager(glacierClient, sqsClient, snsClient);
```

For a list of supported AWS Regions and endpoints, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\.