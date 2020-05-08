# Using the AWS SDK for \.NET with Amazon S3 Glacier<a name="using-aws-sdk-for-dot-net"></a>

The AWS SDK for \.NET API is available in `AWSSDK.dll`\. For information about downloading the AWS SDK for \.NET, go to [Sample Code Libraries](http://aws.amazon.com/sdkfornet/)\. As described in [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md), the AWS SDK for \.NET provides both the high\-level and low\-level APIs\. 

**Note**  
The low\-level API and high\-level API provide thread\-safe clients for accessing S3 Glacier\. As a best practice, your applications should create one client and reuse the client between threads\.

**Topics**
+ [Using the Low\-Level API](#about-low-level-dotnet-api)
+ [Using the High\-Level API](#about-high-level-dotnet-api)
+ [Running Code Examples](#setting-up-and-testing-sdk-dotnet)
+ [Setting the Endpoint](#setting-sdk-dot-net-endpoint)

## Using the Low\-Level API<a name="about-low-level-dotnet-api"></a>

The low\-level `AmazonGlacierClient` class provides all the methods that map to the underlying REST operations of Amazon S3 Glacier \(S3 Glacier\) \( [API Reference for Amazon S3 Glacier](amazon-glacier-api.md)\)\. When calling any of these methods, you must create a corresponding request object and provide a response object in which the method can return a S3 Glacier response to the operation\. 

For example, the `AmazonGlacierClient` class provides the `CreateVault` method to create a vault\. This method maps to the underlying Create Vault REST operation \(see [Create Vault \(PUT vault\)](api-vault-put.md)\)\. To use this method, you must create instances of the `CreateVaultRequest` and `CreateVaultResponse` classes to provide request information and receive a S3 Glacier response as shown in the following C\# code snippet:

```
AmazonGlacierClient client;
client = new AmazonGlacierClient(Amazon.RegionEndpoint.USEast1); 

CreateVaultRequest request = new CreateVaultRequest()
{
  AccountId = "-",
  VaultName = "*** Provide vault name ***"
};

CreateVaultResponse response = client.CreateVault(request);
```

All the low\-level samples in the guide use this pattern\. 

**Note**  
The preceding code segment specifies `AccountId` when creating the request\. However, when using the AWS SDK for \.NET, the `AccountId` in the request is optional, and therefore all the low\-level examples in this guide don't set this value\. The `AccountId` is the AWS Account ID\. This value must match the AWS Account ID associated with the credentials used to sign the request\. You can specify either the AWS Account ID or optionally a '\-', in which case S3 Glacier uses the AWS Account ID associated with the credentials used to sign the request\. If you specify your Account ID, do not include hyphens in it\. When using AWS SDK for \.NET, if you don't provide the account ID, the library sets the account ID to '\-'\. 

## Using the High\-Level API<a name="about-high-level-dotnet-api"></a>

To further simplify your application development, the AWS SDK for \.NET provides the `ArchiveTransferManager` class that implements a higher\-level abstraction for some of the methods in the low\-level API\. It provides useful methods, such as `Upload` and `Download`, for the archive operations\. 

For example, the following C\# code snippet uses the `Upload` high\-level method to upload an archive\. 

```
string vaultName = "examplevault";
string archiveToUpload = "c:\folder\exampleArchive.zip";

var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USEast1);
string archiveId = manager.Upload(vaultName, "archive description", archiveToUpload).ArchiveId;
```

Note that any operations you perform apply to the AWS Region you specified when creating the `ArchiveTransferManager` object\. All the high\-level examples in this guide use this pattern\. 

**Note**  
The high\-level `ArchiveTransferManager` class still needs the low\-level `AmazonGlacierClient` client, which you can pass either explicitly or the `ArchiveTransferManager` creates the client\.

## Running Code Examples<a name="setting-up-and-testing-sdk-dotnet"></a>

The easiest way to get started with the \.NET code examples is to install the AWS SDK for \.NET\. For more information, go to [AWS SDK for \.NET](http://aws.amazon.com/sdkfornet/)\.  

The following procedure outlines steps for you to test the code examples provided in this guide\.


**General Process of Creating \.NET Code Examples \(Using Visual Studio\)**  

|  |  | 
| --- |--- |
| 1 | Create a credentials profile for your AWS credentials as described in the AWS SDK for \.NET topic [Configuring AWS Credentials](http://docs.aws.amazon.com/AWSSdkDocsNET/latest/DeveloperGuide/net-dg-config-creds.html)\.  | 
| 2 |  Create a new Visual Studio project using the *AWS Empty Project* template\.   | 
| 3 | Replace the code in the project file, `Program.cs`, with the code in the section you are reading\.  | 
| 4 |   Run the code\. Verify that the object is created using the AWS Management Console\. For more information about AWS Management Console, go to [http://aws\.amazon\.com/console/](http://aws.amazon.com/console/)\.  | 

## Setting the Endpoint<a name="setting-sdk-dot-net-endpoint"></a>

By default, the AWS SDK for \.NET sets the endpoint to the US West \(Oregon\) Region \(`https://glacier.us-west-2.amazonaws.com`\)\. You can set the endpoint to other AWS Regions as shown in the following C\# snippets\.

The following snippet shows how to set the endpoint to the US West \(Oregon\) Region \(`us-west-2`\) in the low\-level API\.

**Example**  

```
AmazonGlacierClient client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2);
```

The following snippet shows how to set the endpoint to the US West \(Oregon\) Region in the high\-level API\.

```
var manager = new ArchiveTransferManager(Amazon.RegionEndpoint.USWest2);
```

For a current list of supported AWS Regions and endpoints, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\.