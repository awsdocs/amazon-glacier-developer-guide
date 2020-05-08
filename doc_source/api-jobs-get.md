# List Jobs \(GET jobs\)<a name="api-jobs-get"></a>

## Description<a name="api-jobs-get-description"></a>

This operation lists jobs for a vault, including jobs that are in\-progress and jobs that have recently finished\. 

**Note**  
Amazon S3 Glacier \(S3 Glacier\) retains recently completed jobs for a period before deleting them; however, it eventually removes completed jobs\. The output of completed jobs can be retrieved\. Retaining completed jobs for a period of time after they have completed enables you to get a job output in the event you miss the job completion notification, or your first attempt to download it fails\. For example, suppose that you start an archive retrieval job to download an archive\. After the job completes, you start to download the archive but encounter a network error\. In this scenario, you can retry and download the archive while the job exists\. 

The `List Jobs` operation supports pagination\. You should always check the response `Marker` field\. If there are no more jobs to list, the `Marker` field is set to `null`\. If there are more jobs to list, the `Marker` field is set to a non\-null value, which you can use to continue the pagination of the list\. To return a list of jobs that begins at a specific job, set the `marker` request parameter to the `Marker` value for that job that you obtained from a previous `List Jobs` request\.

You can set a maximum limit for the number of jobs returned in the response by specifying the `limit` parameter in the request\. The default limit is 50\. The number of jobs returned might be fewer than the limit, but the number of returned jobs never exceeds the limit\.

Additionally, you can filter the jobs list returned by specifying the optional `statuscode` parameter or `completed` parameter, or both\. Using the `statuscode` parameter, you can specify to return only jobs that match either the `InProgress`, `Succeeded`, or `Failed` status\. Using the `completed` parameter, you can specify to return only jobs that were completed \(`true`\) or jobs that were not completed \(`false`\)\.

## Requests<a name="api-jobs-get-requests"></a>

### Syntax<a name="api-jobs-get-requests-syntax"></a>

 To return a list of jobs of all types, send a `GET` request to the URI of the vault's `jobs` subresource\.

```
1. GET /AccountId/vaults/VaultName/jobs HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-jobs-get-requests-parameters"></a>


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| completed  |  The state of the jobs to return\. You can specify `true` or `false`\. Type: Boolean Constraints: None  |  No  | 
|  limit  |  The maximum number of jobs to be returned\. The default limit is 50\. The number of jobs returned might be fewer than the specified limit, but the number of returned jobs never exceeds the limit\. Type: String Constraints: Minimum integer value of 1\. Maximum integer value of 50\.  |  No | 
| marker  |  An opaque string used for pagination that specifies the job at which the listing of jobs should begin\. You get the `marker` value from a previous `List Jobs` response\. You only need to include the `marker` if you are continuing the pagination of the results started in a previous `List Jobs` request\.  Type: String Constraints: None  |  No  | 
| statuscode  |  The type of job status to return\.  Type: String Constraints: One of the values `InProgress`, `Succeeded`, or `Failed`\.  |  No  | 

### Request Headers<a name="api-jobs-get-requests-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Request Body<a name="api-jobs-get-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-jobs-get-responses"></a>

### Syntax<a name="api-jobs-get-responses-syntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Location: Location 
Content-Type: application/json
Content-Length: Length

{
    "JobList": [
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
    ],
    "Marker": "string"
}
```

### Response Headers<a name="api-jobs-get-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-jobs-get-responses-elements"></a>

The response body contains the following JSON fields\.

**JobList**  
A list of job objects\. Each job object contains metadata describing the job\.  
*Type*: Array of [GlacierJobDescription](api-GlacierJobDescription.md) objects

**Marker**  
An opaque string that represents where to continue pagination of the results\. You use the `marker` value in a new` List Jobs` request to obtain more jobs in the list\. If there are no more jobs to list, this value is `null`\.   
*Type*: String

### Errors<a name="api-jobs-get-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-jobs-get-examples"></a>

The following examples demonstrate how to return information about vault jobs\. The first example returns a list of two jobs, and the second example returns a subset of jobs\.

### Example: Return All Jobs<a name="api-jobs-get-example-requestExample1"></a>

#### Example Request<a name="api-jobs-get-example-request"></a>

The following `GET` request returns the jobs for a vault\. 

```
1. GET /-/vaults/examplevault/jobs  HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-jobs-get-example-response"></a>

The following response includes an archive retrieval job and an inventory retrieval job that contains a marker used to continue pagination of the vault inventory retrieval\. The response also shows the `Marker` field set to `null`, which indicates there are no more jobs to list\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT 
 4. Content-Type: application/json
 5. Content-Length: 1444
 6. 
 7. {
 8.   "JobList": [
 9.     {
10.       "Action": "ArchiveRetrieval",
11.       "ArchiveId": "BDfaUQul0dVzYwAMr8YSa_6_8abbhZq-i1oT69g8ByClfJyBgAGBkWl2QbF5os851P7Y7KdZDOHWJIn4rh1ZHaOYD3MgFhK_g0oDPesW34uHQoVGwoIqubf6BgUEfQm_wrU4Jlm3cA",
12.       "ArchiveSizeInBytes": 1048576,
13.       "ArchiveSHA256TreeHash": "25499381569ab2f85e1fd0eb93c5406a178ab77c5933056eb5d6e7d4adda609b",
14.       "Completed": true,
15.       "CompletionDate": "2012-05-01T00:00:09.304Z",
16.       "CreationDate": "2012-05-01T00:00:06.663Z",
17.       "InventorySizeInBytes": null,
18.       "JobDescription": null,
19.       "JobId": "hDe9t9DTHXqFw8sBGpLQQOmIM0-JrGtu1O_YFKLnzQ64548qJc667BRWTwBLZC76Ygy1jHYruqXkdcAhRsh0hYv4eVRU",
20.       "RetrievalByteRange": "0-1048575",
21.       "SHA256TreeHash": "25499381569ab2f85e1fd0eb93c5406a178ab77c5933056eb5d6e7d4adda609b",
22.       "SNSTopic": null,
23.       "StatusCode": "Succeeded",
24.       "StatusMessage": "Succeeded",
25.       "Tier": "Bulk",
26.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
27.     },
28.     {
29.       "Action": "InventoryRetrieval",
30.       "ArchiveId": null,
31.       "ArchiveSizeInBytes": null,
32.       "ArchiveSHA256TreeHash": null,
33.       "Completed": true,
34.       "CompletionDate": "2013-05-11T00:25:18.831Z",
35.       "CreationDate": "2013-05-11T00:25:14.981Z",
36.       "InventorySizeInBytes": 1988,
37.       "JobDescription": null,
38.       "JobId": "2cvVOnBL36btzyP3pobwIceiaJebM1bx9vZOOUtmNAr0KaVZ4WkWgVjiPldJ73VU7imlm0pnZriBVBebnqaAcirZq_C5",
39.       "RetrievalByteRange": null,
40.       "SHA256TreeHash": null,
41.       "SNSTopic": null,
42.       "StatusCode": "Succeeded",
43.       "StatusMessage": "Succeeded",
44.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
45.       "InventoryRetrievalParameters": {
46.           "StartDate": "2013-11-12T13:43:12Z",
47.           "EndDate": "2013-11-20T08:12:45Z", 
48.           "Limit": "120000",
49.           "Format": "JSON",
50.           "Marker": "vyS0t2jHQe5qbcDggIeD50chS1SXwYMrkVKo0KHiTUjEYxBGCqRLKaiySzdN7QXGVVV5XZpNVG67pCZ_uykQXFMLaxOSu2hO_-5C0AtWMDrfo7LgVOyfnveDRuOSecUo3Ueq7K0"
51.     }
52.   ],
53.   "Marker": null  
54. }
```

### Example: Return a Partial List of Jobs<a name="api-jobs-get-example-request-example2"></a>

#### Example Request<a name="api-jobs-get-example-request2"></a>

The following `GET` request returns the job specified by the `marker` parameter\. Setting the `limit` parameter as `2` specifies that up to two jobs are returned\.

```
1. GET /-/vaults/examplevault/jobs?marker=HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID&limit=2  HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-jobs-get-example-response2"></a>

The following response shows two jobs returned and the `Marker` field set to a non\-null value that can be used to continue pagination of the job list\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT 
 4. Content-Type: application/json
 5. Content-Length: 1744
 6. 
 7. {
 8.   "JobList": [
 9.     {
10.       "Action": "ArchiveRetrieval",
11.       "ArchiveId": "58-3KpZfcMPUznvMZNPaKyJx9wODCsWTnqcjtx2CjKZ6b-XgxEuA8yvZOYTPQfd7gWR4GRm2XR08gcnWbLV4VPV_kDWtZJKi0TFhKKVPzwrZnA4-FXuIBfViYUIVveeiBE51FO4bvg",
12.       "ArchiveSizeInBytes": 8388608,
13.       "ArchiveSHA256TreeHash": "106086b256ddf0fedf3d9e72f461d5983a2566247ebe7e1949246bc61359b4f4",
14.       "Completed": true,
15.       "CompletionDate": "2012-05-01T00:25:20.043Z",
16.       "CreationDate": "2012-05-01T00:25:16.344Z",
17.       "InventorySizeInBytes": null,
18.       "JobDescription": "aaabbbccc",
19.       "JobId": "s4MvaNHIh6mOa1f8iY4ioG2921SDPihXxh3Kv0FBX-JbNPctpRvE4c2_BifuhdGLqEhGBNGeB6Ub-JMunR9JoVa8y1hQ",
20.       "RetrievalByteRange": "0-8388607",
21.       "SHA256TreeHash": "106086b256ddf0fedf3d9e72f461d5983a2566247ebe7e1949246bc61359b4f4",
22.       "SNSTopic": null,
23.       "StatusCode": "Succeeded",
24.       "StatusMessage": "Succeeded",
25.       "Tier": "Bulk",
26.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
27.     },
28.     {
29.       "Action": "ArchiveRetrieval",
30.       "ArchiveId": "2NVGpf83U6qB9M2u-Ihh61yoFLRDEoh7YLZWKBn80A2i1xG8uieBwGjAr4RkzOHA0E07ZjtI267R03Z-6Hxd8pyGQkBdciCSH1-Lw63Kx9qKpZbPCdU0uTW_WAdwF6lR6w8iSyKdvw",
31.       "ArchiveSizeInBytes": 1048576,
32.       "ArchiveSHA256TreeHash": "3d2ae052b2978727e0c51c0a5e32961c6a56650d1f2e4ceccab6472a5ed4a0",
33.       "Completed": true,
34.       "CompletionDate": "2012-05-01T16:59:48.444Z",
35.       "CreationDate": "2012-05-01T16:59:42.977Z",
36.       "InventorySizeInBytes": null,
37.       "JobDescription": "aaabbbccc",
38.       "JobId": "CQ_tf6fOR4jrJCL61Mfk6VM03oY8lmnWK93KK4gLig1UPAbZiN3UV4G_5nq4AfmJHQ_dOMLOX5k8ItFv0wCPN0oaz5dG",
39.       "RetrievalByteRange": "0-1048575",
40.       "SHA256TreeHash": "3d2ae052b2978727e0c51c0a5e32961c6a56650d1f2e4ceccab6472a5ed4a0",
41.       "SNSTopic": null,
42.       "StatusCode": "Succeeded",
43.       "StatusMessage": "Succeeded",
44.       "Tier": "Standard",
45.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
46.     }
47.   ],
48.   "Marker": "CQ_tf6fOR4jrJCL61Mfk6VM03oY8lmnWK93KK4gLig1UPAbZiN3UV4G_5nq4AfmJHQ_dOMLOX5k8ItFv0wCPN0oaz5dG"
49. }
```

## Related Sections<a name="related-sections-list-jobs"></a>
+  [Describe Job \(GET JobID\)](api-describe-job-get.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md) 