# Initiate Job \(POST jobs\)<a name="api-initiate-job-post"></a>

This operation initiates the following types of Amazon S3 Glacier \(S3 Glacier\) jobs: 
+ `select`— Perform a select query on an archive
+ `archive-retrieval`— Retrieve an archive
+ `inventory-retrieval`— Inventory a vault

**Topics**
+ [Working with Amazon S3 Glacier Select Jobs](#api-initiate-select-job)
+ [Initializing an Archive or Vault Inventory Retrieval Job](#api-initiate-job-post-description)
+ [Requests](#api-initiate-job-post-requests)
+ [Responses](#api-initiate-job-post-responses)
+ [Examples](#api-initiate-job-post-examples)
+ [Related Sections](#more-info-api-initiate-job-post)

## Working with Amazon S3 Glacier Select Jobs<a name="api-initiate-select-job"></a>

You use a S3 Glacier Select job to perform SQL queries on archive objects\. The archive objects being queried by the select job must be formatted as uncompressed comma\-separated values \(CSV\) files\. For overview information about S3 Glacier Select jobs, see [Querying Archives with S3 Glacier Select](glacier-select.md)\.

When initiating a select job, you do the following:
+ Define an output location for the output of your select query\. This location must be an Amazon S3 bucket in the same AWS Region as the vault containing the archive object being queried\. The AWS account that initiates the job must have permissions to write to the S3 bucket\. You can specify the storage class and encryption for the output objects stored in Amazon S3\. When setting [S3Location](api-S3Location.md), it might be helpful to read the following topics in the Amazon S3 documentation:
  + [PUT Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html) in the *Amazon Simple Storage Service API Reference*
  + [Managing Access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*
  + [Protecting Data Using Server\-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html) in the *Amazon Simple Storage Service Developer Guide*
+ Define the SQL expression to use for the SELECT for your query in [SelectParameters](api-SelectParameters.md)\. For example, you can use expressions like the following examples:
  + The following example expression returns all records from the specified object\.

    ```
    SELECT * FROM archive
    ```
  + Assuming you are not using any headers for data stored in the object, you can specify columns using positional headers\.

    ```
    SELECT s._1, s._2 FROM archive s WHERE s._3 > 100
    ```
  + If you have headers and you set the `fileHeaderInfo` in [CSVInput](api-CSVInput.md) to `Use`, you can specify headers in the query\. \(If you set the `fileHeaderInfo` field to `Ignore`, the first row is skipped for the query\.\) You cannot mix ordinal positions with header column names\. 

    ```
    SELECT s.Id, s.FirstName, s.SSN FROM archive s
    ```

For more information about using SQL with S3 Glacier Select, see [SQL Reference for Amazon S3 Select and S3 Glacier Select](s3-glacier-select-sql-reference.md)\.

When initiating a select job, you can also do the following:
+ Specify the `Expedited` tier to expedite your queries\. For more information, see [Expedited, Standard, and Bulk Tiers](#api-initiate-job-expedited-bulk)\.
+ Specify details about the data serialization format of both the input object being queried and the serialization of the CSV\-encoded query results\.
+ Specify an Amazon Simple Notification Service \(Amazon SNS\) topic to which S3 Glacier can post a notification after the job is completed\. You can specify an SNS topic for each job request\. The notification is sent only after S3 Glacier completes the job\.
+ You can use [Describe Job \(GET JobID\)](api-describe-job-get.md) to obtain job status information while a job is in progress\. However, it is more efficient to use an Amazon SNS notification to determine when a job is complete\.

When working with a select job, you cannot do the following:
+  Call the **GetJobOutput** operation\. Job output is written to the output location\.
+  Use ranged selection\.

For an example of initiating a select job, see [Example Request: Initiate a select job](#api-initiate-job-post-example-select-request)\.

## Initializing an Archive or Vault Inventory Retrieval Job<a name="api-initiate-job-post-description"></a>

Retrieving an archive or a vault inventory are asynchronous operations that require you to initiate a job\. Once started, job cannot be cancelled\. Retrieval is a two\-step process:

1. Initiate a retrieval job by using the [Initiate Job \(POST jobs\)](#api-initiate-job-post) operation\.
**Important**  
A data retrieval policy can cause your initiate retrieval job request to fail with a `PolicyEnforcedException`\. For more information about data retrieval policies, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\. For more information about the `PolicyEnforcedException` exception, see [Error Responses](api-error-responses.md)\.

1. After the job completes, download the bytes using the [Get Job Output \(GET output\)](api-job-output-get.md) operation\. 

The retrieval request is ran asynchronously\. When you initiate a retrieval job, S3 Glacier creates a job and returns a job ID in the response\. When S3 Glacier completes the job, you can get the job output \(archive or inventory data\)\. For information about getting job output, see the [Get Job Output \(GET output\)](api-job-output-get.md) operation\. 

The job must complete before you can get its output\. To determine when a job is complete, you have the following options:
+ **Use an Amazon SNS notification—** You can specify an Amazon SNS topic to which S3 Glacier can post a notification after the job is completed\. You can specify an SNS topic per job request\. The notification is sent only after S3 Glacier completes the job\. In addition to specifying an SNS topic per job request, you can configure vault notifications for a vault so that job notifications are sent for all retrievals\. For more information, see [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)\. 
+ **Get job details—** You can make a [Describe Job \(GET JobID\)](api-describe-job-get.md) request to obtain job status information while a job is in progress\. However, it is more efficient to use an Amazon SNS notification to determine when a job is complete\.

**Note**  
The information you get via notification is same that you get by calling [Describe Job \(GET JobID\)](api-describe-job-get.md)\. 

If for a specific event, you add both the notification configuration on the vault and also specify an SNS topic in your initiate job request, S3 Glacier sends both notifications\. For more information, see [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)\.

### The Vault Inventory<a name="api-initiate-job-post-about-the-vault-inventory"></a>

S3 Glacier updates a vault inventory approximately once a day, starting on the day you first upload an archive to the vault\. If there have been no archive additions or deletions to the vault since the last inventory, the inventory date is not updated\. When you initiate a job for a vault inventory, S3 Glacier returns the last inventory it generated, which is a point\-in\-time snapshot and not real\-time data\. 

After S3 Glacier creates the first inventory for the vault, it typically takes half a day and up to a day before that inventory is available for retrieval\. 

You might not find it useful to retrieve a vault inventory for each archive upload\. However, suppose that you maintain a database on the client\-side associating metadata about the archives you upload to S3 Glacier\. Then, you might find the vault inventory useful to reconcile information, as needed, in your database with the actual vault inventory\. For more information about the data fields returned in an inventory job output, see [Response Body](api-job-output-get.md#api-job-output-get-responses-elements)\.

### Range Inventory Retrieval<a name="api-initiate-job-post-vault-inventory-list-filtering"></a>

 You can limit the number of inventory items retrieved by filtering on the archive creation date or by setting a limit\.

**Filtering by Archive Creation Date**  
You can retrieve inventory items for archives created between `StartDate` and `EndDate` by specifying values for these parameters in the **Initiate Job** request\. Archives created on or after the `StartDate` and before the `EndDate` are returned\. If you provide only the `StartDate` without the `EndDate`, you retrieve the inventory for all archives created on or after the `StartDate`\. If you provide only the `EndDate` without the `StartDate`, you get back the inventory for all archives created before the `EndDate`\.

**Limiting Inventory Items per Retrieval**  
 You can limit the number of inventory items returned by setting the `Limit` parameter in the **Initiate Job** request\. The inventory job output contains inventory items up to the specified `Limit`\. If there are more inventory items available, the result is paginated\. After a job is complete, you can use the [Describe Job \(GET JobID\)](api-describe-job-get.md) operation to get a marker that you use in a subsequent **Initiate Job** request\. The marker indicates the starting point to retrieve the next set of inventory items\. You can page through your entire inventory by repeatedly making **Initiate Job** requests with the marker from the previous **Describe Job** output\. You do so until you get a marker from **Describe Job** that returns null, indicating that there are no more inventory items available\.

You can use the `Limit` parameter together with the date range parameters\.

### Ranged Archive Retrieval<a name="api-initiate-job-post-"></a>

You can initiate archive retrieval for the whole archive or a range of the archive\. In the case of ranged archive retrieval, you specify a byte range to return or the whole archive\. The range specified must be megabyte \(MB\) aligned\. In other words, the range start value must be divisible by 1 MB and the range end value plus 1 must be divisible by 1 MB or equal the end of the archive\. If the ranged archive retrieval is not megabyte\-aligned, this operation returns a `400` response\. Furthermore, to ensure that you get checksum values for data you download using **Get Job Output** \([Get Job Output \(GET output\)](api-job-output-get.md)\), the range must be tree\-hash aligned\. For more information about tree\-hash aligned ranges, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\.

### Expedited, Standard, and Bulk Tiers<a name="api-initiate-job-expedited-bulk"></a>

When initiating a select or an archive retrieval job, you can specify one of the following options in the `Tier` field of the request body: 
+ **`Expedited`** – Expedited allows you to quickly access your data when occasional urgent requests for a subset of archives are required\. For all but the largest archives \(250 MB\+\), data accessed using the Expedited tier is typically made available within 1–5 minutes\.
+ **`Standard`** – Standard allows you to access any of your archives within several hours\. Data accessed using the Standard tier typically made available within 3–5 hours\. This option is the default one for job requests that don't specify the tier option\.
+ **`Bulk`** – Bulk is the lowest\-cost tier for S3 Glacier, enabling you to retrieve large amounts, even petabytes, of data inexpensively in a day\. Data accessed using the Bulk tier is typically made available within 5–12 hours\.

For more information about expedited and bulk retrievals, see [Retrieving S3 Glacier Archives](downloading-an-archive-two-steps.md)\.

## Requests<a name="api-initiate-job-post-requests"></a>

To initiate a job, you use the HTTP `POST` method and scope the request to the vault's `jobs` subresource\. You specify details of the job request in the JSON document of your request\. The job type is specified with the `Type` field\. Optionally, you can specify an `SNSTopic` field to indicate an Amazon SNS topic to which S3 Glacier can post notification after it completes the job\.

**Note**  
To post a notification to Amazon SNS, you must create the topic yourself if it doesn't already exist\. S3 Glacier doesn't create the topic for you\. The topic must have permissions to receive publications from a S3 Glacier vault\. S3 Glacier doesn't verify if the vault has permission to publish to the topic\. If the permissions are not configured appropriately, you might not receive notification even after the job completes\.

### Syntax<a name="api-initiate-job-post-requests-syntax"></a>

The following is the request syntax for initiating a job\.

```
 1. POST /AccountId/vaults/VaultName/jobs HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. x-amz-glacier-version: 2012-06-01
 6. 
 7. {
 8.    "jobParameters": { 
 9.       "ArchiveId": "string",
10.       "Description": "string",
11.       "Format": "string",
12.       "InventoryRetrievalParameters": { 
13.          "EndDate": "string",
14.          "Limit": "string",
15.          "Marker": "string",
16.          "StartDate": "string"
17.       },
18.       "OutputLocation": { 
19.          "S3": { 
20.             "AccessControlList": [ 
21.                { 
22.                   "Grantee": { 
23.                      "DisplayName": "string",
24.                      "EmailAddress": "string",
25.                      "ID": "string",
26.                      "Type": "string",
27.                      "URI": "string"
28.                   },
29.                   "Permission": "string"
30.                }
31.             ],
32.             "BucketName": "string",
33.             "CannedACL": "string",
34.             "Encryption": { 
35.                "EncryptionType": "string",
36.                "KMSContext": "string",
37.                "KMSKeyId": "string"
38.             },
39.             "Prefix": "string",
40.             "StorageClass": "string",
41.             "Tagging": { 
42.                "string" : "string" 
43.             },
44.             "UserMetadata": { 
45.                "string" : "string" 
46.             }
47.          }
48.       },
49.       "RetrievalByteRange": "string",
50.       "SelectParameters": { 
51.          "Expression": "string",
52.          "ExpressionType": "string",
53.          "InputSerialization": { 
54.             "csv": { 
55.                "Comments": "string",
56.                "FieldDelimiter": "string",
57.                "FileHeaderInfo": "string",
58.                "QuoteCharacter": "string",
59.                "QuoteEscapeCharacter": "string",
60.                "RecordDelimiter": "string"
61.             }
62.          },
63.          "OutputSerialization": { 
64.             "csv": { 
65.                "FieldDelimiter": "string",
66.                "QuoteCharacter": "string",
67.                "QuoteEscapeCharacter": "string",
68.                "QuoteFields": "string",
69.                "RecordDelimiter": "string"
70.             }
71.          }
72.       },
73.       "SNSTopic": "string",
74.       "Tier": "string",
75.       "Type": "string"
76.    }
77. }
```

**Note**  
 The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\. 

### Request Body<a name="api-initiate-job-post-select-request-body"></a>

The request accepts the following data in JSON format in the body of the request\.

jobParameters  
Provides options for specifying job information\.  
*Type*: [jobParameters](api-jobParameters.md) object  
*Required*: Yes 

## Responses<a name="api-initiate-job-post-responses"></a>

S3 Glacier creates the job\. In the response, it returns the URI of the job\.

### Syntax<a name="api-initiate-job-post-response-syntax"></a>

```
1. HTTP/1.1 202 Accepted
2. x-amzn-RequestId: x-amzn-RequestId
3. Date: Date
4. Location: location
5. x-amz-job-id: jobId
6. x-amz-job-output-path: jobOutputPath
```

### Response Headers<a name="api-initiate-job-post-responses-headers"></a>


| Header | Description | 
| --- | --- | 
| Location |  The relative URI path of the job\. You can use this URI path to find the job status\. For more information, see [Describe Job \(GET JobID\)](api-describe-job-get.md)\. Type: String Default: None  | 
| x\-amz\-job\-id |  The ID of the job\. This value is also included as part of the `Location` header\.  Type: String Default: None  | 
| x\-amz\-job\-output\-path |  This header is only returned for select job types\. The path to the location of where the select results are stored\.  Type: String Default: None  | 

### Response Body<a name="api-initiate-job-post-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-initiate-job-post-responses-errors"></a>

This operation includes the following error or errors, in addition to the possible errors common to all Amazon S3 Glacier operations\. For information about Amazon S3 Glacier errors and a list of error codes, see [Error Responses](api-error-responses.md)\.


| Code | Description | HTTP Status Code | Type | 
| --- | --- | --- | --- | 
| InsufficientCapacityException | Returned if there is insufficient capacity to process this expedited request\. This error only applies to expedited retrievals and not to standard or bulk retrievals\. | 503 Service Unavailable | Server | 

## Examples<a name="api-initiate-job-post-examples"></a>

### Example Request: Initiate an archive retrieval job<a name="api-initiate-job-post-example-request"></a>

```
 1. POST /-/vaults/examplevault/jobs HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-glacier-version: 2012-06-01
 5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 6. 
 7. {
 8.   "Type": "archive-retrieval",
 9.   "ArchiveId": "NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId",
10.   "Description": "My archive description",
11.   "SNSTopic": "arn:aws:sns:us-west-2:111111111111:Glacier-ArchiveRetrieval-topic-Example",
12.   "Tier" : "Bulk"
13. }
```

The following is an example of the body of a request that specifies a range of the archive to retrieve using the `RetrievalByteRange` field\.

```
{
  "Type": "archive-retrieval",
  "ArchiveId": "NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId",
  "Description": "My archive description",
  "RetrievalByteRange": "2097152-4194303",
  "SNSTopic": "arn:aws:sns:us-west-2:111111111111:Glacier-ArchiveRetrieval-topic-Example",
  "Tier" : "Bulk"
}
```

### Example Response<a name="api-initiate-job-post-example-response"></a>

```
1. HTTP/1.1 202 Accepted
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. Location: /111122223333/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
5. x-amz-job-id: HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
```

### Example Request: Initiate an inventory retrieval job<a name="api-initiate-job-post-example-retrieve-inventory-request"></a>

The following request initiates an inventory retrieval job to get a list of archives from the `examplevault` vault\. The `Format` set to `CSV` in the body of the request indicates that the inventory is returned in CSV format\.

```
 1. POST /-/vaults/examplevault/jobs HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. Content-Type: application/x-www-form-urlencoded
 5. x-amz-glacier-version: 2012-06-01
 6. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 7. 
 8. {
 9.   "Type": "inventory-retrieval",
10.   "Description": "My inventory job",
11.   "Format": "CSV",  
12.   "SNSTopic": "arn:aws:sns:us-west-2:111111111111:Glacier-InventoryRetrieval-topic-Example"
13. }
```

### Example Response<a name="api-initiate-job-post-example-retrieve-inventory-response"></a>

```
1. HTTP/1.1 202 Accepted
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT 
4. Location: /111122223333/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
5. x-amz-job-id: HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
```

### Example Requests: Initiate an inventory retrieval job by using date filtering with a set limit, and a subsequent request to retrieve the next page of inventory items\.<a name="api-initiate-job-post-example-retrieve-inventory-request-filtered"></a>

The following request initiates a vault inventory retrieval job by using date filtering and setting a limit\. 

```
 1. {
 2.     "ArchiveId": null, 
 3.     "Description": null, 
 4.     "Format": "CSV", 
 5.     "RetrievalByteRange": null, 
 6.     "SNSTopic": null, 
 7.     "Type": "inventory-retrieval", 
 8.     "InventoryRetrievalParameters": {
 9.         "StartDate": "2013-12-04T21:25:42Z",
10.         "EndDate": "2013-12-05T21:25:42Z", 
11.         "Limit" : "10000"
12.     }, 
13. }
```

The following request is an example of a subsequent request to retrieve the next page of inventory items using a marker obtained from [Describe Job \(GET JobID\)](api-describe-job-get.md)\. 

```
 1. {
 2.     "ArchiveId": null, 
 3.     "Description": null, 
 4.     "Format": "CSV", 
 5.     "RetrievalByteRange": null, 
 6.     "SNSTopic": null, 
 7.     "Type": "inventory-retrieval", 
 8.     "InventoryRetrievalParameters": {
 9.         "StartDate": "2013-12-04T21:25:42Z",
10.         "EndDate": "2013-12-05T21:25:42Z", 
11.         "Limit": "10000",
12.         "Marker": "vyS0t2jHQe5qbcDggIeD50chS1SXwYMrkVKo0KHiTUjEYxBGCqRLKaiySzdN7QXGVVV5XZpNVG67pCZ_uykQXFMLaxOSu2hO_-5C0AtWMDrfo7LgVOyfnveDRuOSecUo3Ueq7K0"
13.     }, 
14. }
```

### Example Request: Initiate a select job<a name="api-initiate-job-post-example-select-request"></a>

The following request initiates a select job\.

```
 1. POST /-/vaults/examplevault/jobs HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. Content-Type: application/x-www-form-urlencoded
 5. x-amz-glacier-version: 2012-06-01
 6. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 7. 
 8. {
 9.    
10.   
11.     "Type": "select",
12.     "ArchiveId": "NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId",
13.     "Description": null,
14.     "SNSTopic": null,
15. 
16.     "Tier": "Bulk",
17.     "SelectParameters": {
18.         "Expression": "select * from archive",
19.         "ExpressionType": "SQL",
20.         "InputSerialization": {
21.             "csv": {
22.                 "Comments": null,
23.                 "FileHeaderInfo": "None",
24.                 "QuoteEscapeCharacter": "\"",
25.                 "RecordDelimiter": "\n",
26.                 "FieldDelimiter": ",",
27.                 "QuoteCharacter": "\""
28.             }
29.         },
30.         "OutputSerialization": {
31.             "csv": {
32.                 "QuoteFields": "AsNeeded",
33.                 "QuoteEscapeCharacter": null,
34.                 "RecordDelimiter": "\n",
35.                 "FieldDelimiter": ",",
36.                 "QuoteCharacter": "\""
37.             }
38.         }
39.     },
40.     "OutputLocation": {
41.         "S3": {
42.             "BucketName": "bucket-name",
43.             "Prefix": "test",
44.             "Encryption": {
45.                 "EncryptionType": "AES256"
46.             },
47.             "CannedACL": "private",
48.             "StorageClass": "STANDARD"
49.         }
50.     }
51. }
```

### Example Response<a name="api-initiate-job-post-example-select-response"></a>

```
1. HTTP/1.1 202 Accepted
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT 
4. Location: /111122223333/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
5. x-amz-job-id: HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
6. x-amz-job-output-path: test/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID/
```

## Related Sections<a name="more-info-api-initiate-job-post"></a>
+ [Describe Job \(GET JobID\)](api-describe-job-get.md)
+ [Get Job Output \(GET output\)](api-job-output-get.md)
+ [SQL Reference for Amazon S3 Select and S3 Glacier Select](s3-glacier-select-sql-reference.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)