# Get Job Output \(GET output\)<a name="api-job-output-get"></a>

## Description<a name="api-job-output-get-description"></a>

This operation downloads the output of the job you initiated using [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\. Depending on the job type you specified when you initiated the job, the output will be either the content of an archive or a vault inventory\. 

You can download all the job output or download a portion of the output by specifying a byte range\. For both archive and inventory retrieval jobs, you should verify the downloaded size against the size returned in the headers from the **Get Job Output** response\.

For archive retrieval jobs, you should also verify that the size is what you expected\. If you download a portion of the output, the expected size is based on the range of bytes you specified\. For example, if you specify a range of `bytes=0-1048575`, you should verify your download size is 1,048,576 bytes\. If you download an entire archive, the expected size is the size of the archive when you uploaded it to Amazon S3 Glacier \(S3 Glacier\)\. The expected size is also returned in the headers from the **Get Job Output** response\.

In the case of an archive retrieval job, depending on the byte range you specify, S3 Glacier returns the checksum for the portion of the data\. To ensure the portion you downloaded is the correct data, compute the checksum on the client, verify that the values match, and verify that the size is what you expected\.

A job ID does not expire for at least 24 hours after S3 Glacier completes the job\. That is, you can download the job output within the 24\-hour period after S3 Glacier completes the job\.

## Requests<a name="api-job-output-get-requests"></a>

### Syntax<a name="api-job-output-get-requests-syntax"></a>

To retrieve a job output, you send the HTTP `GET` request to the URI of the `output` of the specific job\. 

```
1. GET /AccountId/vaults/VaultName/jobs/JobID/output HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. Range: ByteRangeToRetrieve
6. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-job-output-get-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-job-output-get-requests-headers"></a>

This operation uses the following request headers, in addition to the request headers that are common to all operations\. For more information about the common request headers, see [Common Request Headers](api-common-request-headers.md)\.


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| Range  |  The range of bytes to retrieve from the output\. For example, if you want to download the first 1,048,576 bytes, specify the range as `bytes=0-1048575`\. For more information, go to [Range Header Field Definition](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.35)\. The range is relative to any range specified in the Initiate Job request\. By default, this operation downloads the entire output\.  If the job output is large, then you can use the `Range` request header to retrieve a portion of the output\. This allows you to download the entire output in smaller chunks of bytes\. For example, suppose you have 1 GB job output you want to download and you decide to download 128 MB chunks of data at a time, a total of eight Get Job Output requests\. You will use the following process to download the job output: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/api-job-output-get.html) Type: String Default: None Constraints: None  |  No  | 

### Request Body<a name="api-job-output-get-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-job-output-get-responses"></a>

### Syntax<a name="api-job-output-get-response-syntax"></a>

For a retrieval request that returns all of the job data, the job output response returns a `200 OK` response code\. When partial content is requested, for example, if you specified the `Range` header in the request, then the response code `206 Partial Content` is returned\.

```
1. HTTP/1.1 200 OK
2. x-amzn-RequestId: x-amzn-RequestId
3. Date: Date
4. Content-Type: ContentType
5. Content-Length: Length
6. x-amz-sha256-tree-hash: ChecksumComputedByAmazonGlacier
7. 
8. [Body containing job output.]
```

### Response Headers<a name="api-job-output-get-responses-headers"></a>


|  Header  |  Description  | 
| --- | --- | 
| Content\-Range  |  The range of bytes returned by S3 Glacier\. If only partial output is downloaded, the response provides the range of bytes S3 Glacier returned\.  For example, `bytes 0-1048575/8388608` returns the first 1 MB from 8 MB\. For more information about the `Content-Range` header, go to [Content\-Range Header Field Definition](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.16)\. Type: String  | 
| Content\-Type  |  The Content\-Type depends on whether the job output is an archive or a vault inventory\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/api-job-output-get.html) Type: String  | 
| x\-amz\-sha256\-tree\-hash​ |  The checksum of the data in the response\. This header is returned only when retrieving the output for an archive retrieval job\. Furthermore, this header appears when the retrieved data range requested in the Initiate Job request is tree hash aligned and the range to download in the Get Job Output is also tree hash aligned\. For more information about tree hash aligned ranges, see [Receiving Checksums When Downloading Data](checksum-calculations-range.md)\.  For example, if in your Initiate Job request you specified a tree hash aligned range to retrieve \(which includes the whole archive\), then you will receive the checksum of the data you download under the following conditions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/api-job-output-get.html) Type: String  | 

### Response Body<a name="api-job-output-get-responses-elements"></a>

S3 Glacier returns the job output in the response body\. Depending on the job type, the output can be the archive contents or the vault inventory\. In case of a vault inventory, by default the inventory list is returned as the following JSON body\. 

```
 1. {
 2.  "VaultARN": String,
 3.  "InventoryDate": String,
 4.  "ArchiveList": [
 5.       {"ArchiveId": String,
 6.        "ArchiveDescription": String,
 7.        "CreationDate": String,
 8.        "Size": Number,
 9.        "SHA256TreeHash": String
10.       },
11.       ...
12.     ]
13. }
```

If you requested the comma\-separated values \(CSV\) output format when you initiated the vault inventory job, then the vault inventory is returned in CSV format in the body\. The CSV format has five columns "ArchiveId", "ArchiveDescription", "CreationDate", "Size", and "SHA256TreeHash" with the same definitions as the corresponding JSON fields\. 

**Note**  
In the returned CSV format, fields may be returned with the whole field enclosed in double\-quotes\. Fields that contain a comma or double\-quotes are always returned enclosed in double\-quotes\. For example, `my archive description,1` is returned as `"my archive description,1"`\. Double\-quote characters that are within returned double\-quote enclosed fields are *escaped* by preceding them with a backslash character\. For example, `my archive description,1"2` is returned as `"my archive description,1\"2"` and `my archive description,1\"2` is returned as `"my archive description,1\\"2"`\. The backslash character is not escaped\.

The JSON response body contains the following JSON fields\.

**ArchiveDescription**  
The description of an archive\.  
*Type*: String

**ArchiveId**  
The ID of an archive\.  
*Type*: String

**ArchiveList**  
An array of archive metadata\. Each object in the array represents metadata for one archive contained in the vault\.   
*Type*: Array

**CreationDate**  
The UTC date and time the archive was created\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**InventoryDate**  
The UTC date and time of the last inventory for the vault that was completed after changes to the vault\. Even though S3 Glacier prepares a vault inventory once a day, the inventory date is only updated if there have been archive additions or deletions to the vault since the last inventory\.   
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**SHA256TreeHash**  
The tree hash of the archive\.  
*Type*: String

**Size**  
The size in bytes of the archive\.  
*Type*: Number

**VaultARN**  
The Amazon Resource Name \(ARN\) resource from which the archive retrieval was requested\.  
*Type*: String

### Errors<a name="api-job-output-get-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-job-output-get-examples"></a>

The following example shows the request for a job that retrieves an archive\.

### Example 1: Download output<a name="api-job-output-get-examplesEx1"></a>

This example retrieves data prepared by S3 Glacier in response to your initiate archive retrieval job request\.

#### Example Request<a name="api-job-output-get-example-request"></a>

```
1. GET /-/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID/output HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-job-output-get-example-response"></a>

The following is an example response of an archive retrieval job\. Note that the `Content-Type` header is `application/octet-stream` and that `x-amz-sha256-tree-hash` header is included in the response, which means that all the job data is returned\.

```
1. HTTP/1.1 200 OK
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. x-amz-sha256-tree-hash: beb0fe31a1c7ca8c6c04d574ea906e3f97b31fdca7571defb5b44dca89b5af60
4. Date: Wed, 10 Feb 2017 12:00:00 GMT
5. Content-Type: application/octet-stream
6. Content-Length: 1048576
7. 
8. [Archive data.]
```

The following is an example response of an inventory retrieval job\. Note that the `Content-Type` header is `application/json`\. Also note that the response does not include the `x-amz-sha256-tree-hash` header\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 906
 6. 
 7. {
 8.  "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault",
 9.  "InventoryDate": "2011-12-12T14:19:01Z",
10.  "ArchiveList": [
11.    {
12.      "ArchiveId": "DMTmICA2n5Tdqq5BV2z7og-A20xnpAPKt3UXwWxdWsn_D6auTUrW6kwy5Qyj9xd1MCE1mBYvMQ63LWaT8yTMzMaCxB_9VBWrW4Jw4zsvg5kehAPDVKcppUD1X7b24JukOr4mMAq-oA",
13.      "ArchiveDescription": "my archive1",
14.      "CreationDate": "2012-05-15T17:19:46.700Z",
15.      "Size": 2140123,
16.      "SHA256TreeHash": "6b9d4cf8697bd3af6aa1b590a0b27b337da5b18988dbcc619a3e608a554a1e62"
17.    },
18.    {
19.      "ArchiveId": "2lHzwhKhgF2JHyvCS-ZRuF08IQLuyB4265Hs3AXj9MoAIhz7tbXAvcFeHusgU_hViO1WeCBe0N5lsYYHRyZ7rrmRkNRuYrXUs_sjl2K8ume_7mKO_0i7C-uHE1oHqaW9d37pabXrSA",
20.      "ArchiveDescription": "my archive2",
21.      "CreationDate": "2012-05-15T17:21:39.339Z",
22.      "Size": 2140123,
23.      "SHA256TreeHash": "7f2fe580edb35154041fa3d4b41dd6d3adaef0c85d2ff6309f1d4b520eeecda3"
24.    }
25.   ]
26. }
```

### Example 2: Download only partial output<a name="api-job-output-get-examples2"></a>

This example retrieves only a portion of the archive prepared by S3 Glacier in response to your initiate archive retrieval job request\. The request uses the optional `Range` header to retrieve only the first 1,024 bytes\.

#### Example Request<a name="api-job-output-get-example-request2"></a>

```
1. GET /-/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID/output HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Range: bytes=0-1023	
5. x-amz-glacier-version: 2012-06-01
6. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-job-output-get-example-response2"></a>

The following successful response shows the `206 Partial Content` response\. In this case, the response also includes a `Content-Range` header that specifies the range of bytes S3 Glacier returns\.

```
1. HTTP/1.1 206 Partial Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. Content-Range: bytes 0-1023/8388608
5. Content-Type: application/octet-stream
6. Content-Length: 1024
7. 
8. [Archive data.]
```

## Related Sections<a name="related-sections-job-output-archive-retrieval"></a>
+ [Describe Job \(GET JobID\)](api-describe-job-get.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)