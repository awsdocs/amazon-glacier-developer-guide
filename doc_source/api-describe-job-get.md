# Describe Job \(GET JobID\)<a name="api-describe-job-get"></a>

## Description<a name="api-describe-job-get-description"></a>

This operation returns information about a job you previously initiated, including the job initiation date, the user who initiated the job, the job status code/message, and the Amazon Simple Notification Service \(Amazon SNS\) topic to notify after Amazon S3 Glacier \(S3 Glacier\) completes the job\. For more information about initiating a job, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\. 

**Note**  
This operation enables you to check the status of your job\. However, we strongly recommend that you set up an Amazon SNS topic and specify it in your initiate job request so that S3 Glacier can notify the topic after it completes the job\. 

A job ID will not expire for at least 24 hours after S3 Glacier completes the job\. 

## Requests<a name="api-describe-job-get-requests"></a>

### Syntax<a name="api-describe-job-get-requests-syntax"></a>

To obtain information about a job, you use the HTTP `GET` method and scope the request to the specific job\. Note that the relative URI path is the same one that S3 Glacier returned to you when you initiated the job\. 

```
1. GET /AccountID/vaults/VaultName/jobs/JobID HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: date
4. Authorization: signatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

**Note**  
In the request, if you omit the `JobID`, the response returns a list of all active jobs on the specified vault\. For more information about listing jobs, see [List Jobs \(GET jobs\)](api-jobs-get.md)\.

### Request Parameters<a name="api-describe-job-get-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-describe-job-get-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-describe-job-get-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-describe-job-get-responses"></a>

### Syntax<a name="api-describe-job-get-response-syntax"></a>

```
HTTP/1.1 201 Created
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: Length

{
    "Action": "string",
    "ArchiveId": "string",
    "ArchiveSHA256TreeHash": "string",
    "ArchiveSizeInBytes": number,
    "Completed": boolean,
    "CompletionDate": "string",
    "CreationDate": "string",
    "InventoryRetrievalParameters": {
        "EndDate": "string",
        "Format": "string",
        "Limit": "string",
        "Marker": "string",
        "StartDate": "string"
    },
    "InventorySizeInBytes": number,
    "JobDescription": "string",
    "JobId": "string",
    "JobOutputPath": "string",
    "OutputLocation": {
        "S3": {
            "AccessControlList": [
                {
                    "Grantee": {
                        "DisplayName": "string",
                        "EmailAddress": "string",
                        "ID": "string",
                        "Type": "string",
                        "URI": "string"
                    },
                    "Permission": "string"
                }
            ],
            "BucketName": "string",
            "CannedACL": "string",
            "Encryption": {
                "EncryptionType": "string",
                "KMSContext": "string",
                "KMSKeyId": "string"
            },
            "Prefix": "string",
            "StorageClass": "string",
            "Tagging": {
                "string": "string"
            },
            "UserMetadata": {
                "string": "string"
            }
        }
    },
    "RetrievalByteRange": "string",
    "SelectParameters": {
        "Expression": "string",
        "ExpressionType": "string",
        "InputSerialization": {
            "csv": {
                "Comments": "string",
                "FieldDelimiter": "string",
                "FileHeaderInfo": "string",
                "QuoteCharacter": "string",
                "QuoteEscapeCharacter": "string",
                "RecordDelimiter": "string"
            }
        },
        "OutputSerialization": {
            "csv": {
                "FieldDelimiter": "string",
                "QuoteCharacter": "string",
                "QuoteEscapeCharacter": "string",
                "QuoteFields": "string",
                "RecordDelimiter": "string"
            }
        }
    },
    "SHA256TreeHash": "string",
    "SNSTopic": "string",
    "StatusCode": "string",
    "StatusMessage": "string",
    "Tier": "string",
    "VaultARN": "string"
}
```

### Response Headers<a name="api-describe-job-get-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-describe-job-get-responses-elements"></a>

The response body contains the following JSON fields\.

**Action**  
The job type\. It is either `ArchiveRetrieval`, `InventoryRetrieval`, or `Select`\.  
*Type*: String

**ArchiveId**  
The archive ID requested for a select or archive retrieval job\. Otherwise, this field is `null`\.  
*Type*: String

**ArchiveSHA256TreeHash**  
The SHA256 tree hash of the entire archive for an archive retrieval job\. For inventory retrieval jobs, this field is `null`\.  
*Type*: String

**ArchiveSizeInBytes**  
For an `ArchiveRetrieval` job, this is the size in bytes of the archive being requested for download\. For the `InventoryRetrieval` job, the value is `null`\.  
*Type*: Number

**Completed**  
The job status\. When an archive or inventory retrieval job is completed, you get the job's output using the [Get Job Output \(GET output\)](api-job-output-get.md)\.  
*Type*: Boolean

**CompletionDate**  
The Universal Coordinated Time \(UTC\) time that the job request completed\. While the job is in progress, the value is null\.  
*Type*: String

**CreationDate**  
The UTC time that the job was created\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**InventoryRetrievalParameters**  
Input parameters used for a range inventory retrieval\.  
*Type*: [InventoryRetrievalJobInput](api-InventoryRetrievalJobInput.md) object

**InventorySizeInBytes**  
For an `InventoryRetrieval` job, this is the size in bytes of the inventory requested for download\. For the `ArchiveRetrieval` or `Select` job, the value is `null`\.  
*Type*: Number

**JobDescription**  
The job description you provided when you initiated the job\.  
*Type*: String

**JobId**  
The ID that identifies the job in S3 Glacier\.  
*Type*: String

**JobOutputPath**  
Contains the job output location\.  
*Type*: String

**OutputLocation**  
 An object that contains information about the location where the select job results and errors are stored\.   
*Type*: [OutputLocation](api-OutputLocation.md) object

**RetrievalByteRange**  
The retrieved byte range for archive retrieval jobs in the form "*StartByteValue*\-*EndByteValue*\." If you don't specify a range in the archive retrieval, then the whole archive is retrieved; also *StartByteValue* equals 0, and *EndByteValue* equals the size of the archive minus 1\. For inventory retrieval or select jobs, this field is `null`\.   
*Type*: String

**SelectParameters**  
An object that contains information about the parameters used for a select\.  
*Type*: [SelectParameters](api-SelectParameters.md) object

**SHA256TreeHash**  
The SHA256 tree hash value for the requested range of an archive\. If the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) request for an archive specified a tree\-hash aligned range, then this field returns a value\. For more information about tree\-hash alignment for archive range retrievals, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\.  
For the specific case when the whole archive is retrieved, this value is the same as the `ArchiveSHA256TreeHash` value\.   
This field is `null` in the following situations:  
+ Archive retrieval jobs that specify a range that is not tree\-hash aligned\.
+ Archival jobs that specify a range that is equal to the whole archive and the job status is `InProgress`\. 
+ Inventory jobs\.
+ Select jobs\.
*Type*: String

**SNSTopic**  
An Amazon SNS topic that receives notification\.  
*Type*: String

**StatusCode**  
The code indicating the status of the job\.  
*Valid Values*: `InProgress` \| `Succeeded` \| `Failed`  
*Type*: String

**StatusMessage**  
A friendly message that describes the job status\.  
*Type*: String

**Tier**  
The data access tier to use for the select or archive retrieval\.  
*Valid Values*: `Bulk` \| `Expedited` \| `Standard`  
*Type*: String

**VaultARN**  
The Amazon Resource Name \(ARN\) of the vault of which the job is a subresource\.  
*Type*: String

### Errors<a name="api-describe-job-get-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-describe-job-get-examples"></a>

The following example shows the request for a job that retrieves an archive\.

### Example Request: Get job description<a name="api-describe-job-get-example-request"></a>

```
1. GET /-/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-describe-job-get-example-response"></a>

The response body includes JSON that describes the specified job\. Note that for both the inventory retrieval and archive retrieval jobs, the JSON fields are the same\. However, when a field doesn't apply to the type of job, its value is `null`\. The following is an example response for an archive retrieval job\. Note the following:
+ The `Action` field value is `ArchiveRetrieval`\. 
+ The `ArchiveSizeInBytes` field shows the size of the archive requested in the archive retrieval job\. 
+ The `ArchiveSHA256TreeHash` field shows the SHA256 tree hash for the entire archive\.
+ The `RetrievalByteRange` field shows the range requested in the Initiate Job request\. In this example, the whole archive is requested\.
+ The `SHA256TreeHash` field shows the SHA256 tree hash for the range requested in the Initiate Job request\. In this example, it is the same as the `ArchiveSHA256TreeHash` field, which means that the whole archive was requested\.
+ The `InventorySizeInBytes` field value is `null` because it does not apply to an archive retrieval job\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 419
 6. {
 7.   "Action": "ArchiveRetrieval",
 8.   "ArchiveId": "NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId",
 9.   "ArchiveSizeInBytes": 16777216,
10.   "ArchiveSHA256TreeHash": "beb0fe31a1c7ca8c6c04d574ea906e3f97b31fdca7571defb5b44dca89b5af60",
11.   "Completed": false,
12.   "CompletionDate": null,
13.   "CreationDate": "2012-05-15T17:21:39.339Z",
14.   "InventorySizeInBytes": null,
15.   "JobDescription": "My ArchiveRetrieval Job",
16.   "JobId": "HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID",
17.   "RetrievalByteRange": "0-16777215",
18.   "SHA256TreeHash": "beb0fe31a1c7ca8c6c04d574ea906e3f97b31fdca7571defb5b44dca89b5af60",
19.   "SNSTopic": "arn:aws:sns:us-west-2:012345678901:mytopic",
20.   "StatusCode": "InProgress",
21.   "StatusMessage": "Operation in progress.",
22.   "Tier": "Bulk",
23.   "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
24. }
```

The following is an example response for an inventory retrieval job\. Note the following:
+ The `Action` field value is `InventoryRetrieval`\. 
+ The `ArchiveSizeInBytes`, `ArchiveSHA256TreeHash`, and `RetrievalByteRange` field values are null because these fields do not apply to an inventory retrieval job\. 
+ The `InventorySizeInBytes` field value is `null` because the job is still in progress, and has not fully prepared the inventory for download\. If the job was completed before your describe job request, this field would give you the size of the output\.

```
{
   "Action": "InventoryRetrieval",
   "ArchiveId": null,
   "ArchiveSizeInBytes": null,
   "ArchiveSHA256TreeHash": null,
   "Completed": false,
   "CompletionDate": null,
   "CreationDate": "2012-05-15T23:18:13.224Z",
   "InventorySizeInBytes": null,
   "JobDescription": "Inventory Description",
   "JobId": "HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID",
   "RetrievalByteRange": null,
   "SHA256TreeHash": null,
   "SNSTopic": "arn:aws:sns:us-west-2:012345678901:mytopic",
   "StatusCode": "InProgress",
   "StatusMessage": "Operation in progress.",
   "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
}
```

The following is an example response for a completed inventory retrieval job that contains a marker used to continue pagination of the vault inventory retrieval\. 

```
{ 
    "Action": "InventoryRetrieval", 
    "ArchiveId": null, 
    "ArchiveSHA256TreeHash": null, 
    "ArchiveSizeInBytes": null, 
    "Completed": true, 
    "CompletionDate": "2013-12-05T21:51:13.591Z", 
    "CreationDate": "2013-12-05T21:51:12.281Z", 
    "InventorySizeInBytes": 777062, 
    "JobDescription": null, 
    "JobId": "sCC2RZNBF2nildYD_roe0J9bHRdPQUbDRkmTdg-mXi2u3lc49uW6TcEhDF2D9pB2phx-BN30JaBru7PMyOlfXHdStzu8", 
    "NextInventoryRetrievalMarker": null, 
    "RetrievalByteRange": null, 
    "SHA256TreeHash": null, 
    "SNSTopic": null, 
    "StatusCode": "Succeeded", 
    "StatusMessage": "Succeeded", 
    "Tier": "Bulk",
    "VaultARN": "arn:aws:glacier-devo:us-west-2:836579025725:vaults/inventory-icecube-2", 
    "InventoryRetrievalParameters": {
        "StartDate": "2013-11-12T13:43:12Z",
        "EndDate": "2013-11-20T08:12:45Z", 
        "Limit": "120000",
        "Format": "JSON",
        "Marker": "vyS0t2jHQe5qbcDggIeD50chS1SXwYMrkVKo0KHiTUjEYxBGCqRLKaiySzdN7QXGVVV5XZpNVG67pCZ_uykQXFMLaxOSu2hO_-5C0AtWMDrfo7LgVOyfnveDRuOSecUo3Ueq7K0"
    },   
}
```

## Related Sections<a name="related-sections-describe-job-get"></a>
+ [Get Job Output \(GET output\)](api-job-output-get.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)