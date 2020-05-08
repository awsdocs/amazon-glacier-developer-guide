# Upload Part \(PUT uploadID\)<a name="api-upload-part"></a>

## Description<a name="api-job-get-description"></a>

This multipart upload operation uploads a part of an archive\. You can upload archive parts in any order because in your Upload Part request you specify the range of bytes in the assembled archive that will be uploaded in this part\. You can also upload these parts in parallel\. You can upload up to 10,000 parts for a multipart upload\.

For information about multipart upload, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

 

Amazon S3 Glacier \(S3 Glacier\) rejects your upload part request if any of the following conditions is true:
+ **SHA256 tree hash does not match—**To ensure that part data is not corrupted in transmission, you compute a SHA256 tree hash of the part and include it in your request\. Upon receiving the part data, S3 Glacier also computes a SHA256 tree hash\. If the two hash values don't match, the operation fails\. For information about computing a SHA256 tree hash, see [Computing Checksums](checksum-calculations.md)\.
+ **SHA256 linear hash does not match—**Required for authorization, you compute a SHA256 linear hash of the entire uploaded payload and include it in your request\. For information about computing a SHA256 linear hash, see [Computing Checksums](checksum-calculations.md)\.
+ **Part size does not match—**The size of each part except the last must match the size that is specified in the corresponding [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md) request\. The size of the last part must be the same size as, or smaller than, the specified size\.
**Note**  
If you upload a part whose size is smaller than the part size you specified in your initiate multipart upload request and that part is not the last part, then the upload part request will succeed\. However, the subsequent Complete Multipart Upload request will fail\.
+ **Range does not align—**The byte range value in the request does not align with the part size specified in the corresponding initiate request\. For example, if you specify a part size of 4194304 bytes \(4 MB\), then 0 to 4194303 bytes \(4 MB —1\) and 4194304 \(4 MB\) to 8388607 \(8 MB —1\) are valid part ranges\. However, if you set a range value of 2 MB to 6 MB, the range does not align with the part size and the upload will fail\.

This operation is idempotent\. If you upload the same part multiple times, the data included in the most recent request overwrites the previously uploaded data\.

## Requests<a name="api-job-get-requests"></a>

You send this HTTP `PUT` request to the URI of the upload ID that was returned by your Initiate Multipart Upload request\. S3 Glacier uses the upload ID to associate part uploads with a specific multipart upload\. The request must include a SHA256 tree hash of the part data \(`x-amz-SHA256-tree-hash` header\), a SHA256 linear hash of the entire payload \(`x-amz-content-sha256` header\), the byte range \(`Content-Range` header\), and the length of the part in bytes \(`Content-Length` header\)\. 

### Syntax<a name="api-job-get-requests-syntax"></a>

```
 1. PUT /AccountId/vaults/VaultName/multipart-uploads/uploadID HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. Content-Range: ContentRange
 6. Content-Length: PayloadSize
 7. Content-Type: application/octet-stream
 8. x-amz-sha256-tree-hash: Checksum of the part
 9. x-amz-content-sha256: Checksum of the entire payload			
10. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-upload-part-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-upload-part-requests-headers"></a>

This operation uses the following request headers, in addition to the request headers that are common to all operations\. For more information about the common request headers, see [Common Request Headers](api-common-request-headers.md)\.


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| Content\-Length  |  Identifies the length of the part in bytes\.  Type: String Default: None Constraints: None  |  No | 
| Content\-Range  |  Identifies the range of bytes in the assembled archive that will be uploaded in this part\. S3 Glacier uses this information to assemble the archive in the proper sequence\. The format of this header follows [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.16)\. An example header is `Content-Range:bytes 0-4194303/*`\. Type: String Default: None Constraints: The range cannot be greater than the part size that you specified when you initiated the multipart upload\.   |  Yes | 
|  x\-amz\-content\-sha256  |  The SHA256 checksum \(a linear hash\) of the uploaded payload\. This is not the same value as you specify in the `x-amz-sha256-tree-hash` header\. Type: String Default: None Constraints: None  |  Yes | 
|  x\-amz\-sha256\-tree\-hash​  |  Specifies a SHA256 tree hash of the data being uploaded\. For information about computing a SHA256 tree hash, see [Computing Checksums](checksum-calculations.md)\. Type: String Default: None Constraints: None  |  Yes | 

### Request Body<a name="api-job-get-requests-elements"></a>

 The request body contains the data to upload\. 

## Responses<a name="api-job-get-responses"></a>

Upon a successful part upload, S3 Glacier returns a `204 No Content` response\. 

### Syntax<a name="api-job-get-response-syntax"></a>

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: x-amzn-RequestId
3. Date: Date
4. x-amz-sha256-tree-hash: ChecksumComputedByAmazonGlacier
```

### Response Headers<a name="api-job-get-responses-headers"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description  | 
| --- | --- | 
|  x\-amz\-sha256\-tree\-hash​  |  The SHA256 tree hash that S3 Glacier computed for the uploaded part\. Type: String  | 

### Response Body<a name="api-job-get-responses-elements"></a>

This operation does not return a response body\.

## Example<a name="api-job-get-examples"></a>

The following request uploads a 4 MB part\. The request sets the byte range to make this the first part in the archive\. 

### Example Request<a name="api-job-get-examples-sample-request"></a>

The example sends an HTTP `PUT` request to upload a 4 MB part\. The request is sent to the URI of the Upload ID that was returned by the Initiate Multipart Upload request\. The `Content-Range` header identifies the part as the first 4 MB data part of the archive\.

```
1. PUT /-/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. Content-Range:bytes 0-4194303/*
5. x-amz-sha256-tree-hash:c06f7cd4baacb087002a99a5f48bf953
6. x-amz-content-sha256:726e392cb4d09924dbad1cc0ba3b00c3643d03d14cb4b823e2f041cff612a628
7. Content-Length: 4194304
8. Authorization: Authorization=AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-glacier-version,Signature=16b9a9e220a37e32f2e7be196b4ebb87120ca7974038210199ac5982e792cace
```

To upload the next part, the procedure is the same; however, you must calculate a new SHA256 tree hash of the part you are uploading and also specify a new byte range to indicate where the part will go in the final assembly\. The following request uploads another part using the same upload ID\. The request specifies the next 4 MB of the archive after the previous request and a part size of 4 MB\.

```
1. PUT /-/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. Content-Range:bytes 4194304-8388607/*
5. Content-Length: 4194304
6. x-amz-sha256-tree-hash:f10e02544d651e2c3ce90a4307427493
7. x-amz-content-sha256:726e392cb4d09924dbad1cc0ba3b00c3643d03d14cb4b823e2f041cff612a628
8. x-amz-glacier-version: 2012-06-01
9. Authorization: Authorization=AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20120525/us-west-2/glacier/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-glacier-version, Signature=16b9a9e220a37e32f2e7be196b4ebb87120ca7974038210199ac5982e792cace
```

The parts can be uploaded in any order; S3 Glacier uses the range specification for each part to determine the order in which to assemble them\.

### Example Response<a name="api-job-get-examples-sample-response"></a>

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. x-amz-sha256-tree-hash: c06f7cd4baacb087002a99a5f48bf953
4. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-upload-part"></a>
+ [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)
+ [Upload Part \(PUT uploadID\)](#api-upload-part)
+ [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)
+ [Abort Multipart Upload \(DELETE uploadID\)](api-multipart-abort-upload.md)
+ [List Multipart Uploads \(GET multipart\-uploads\)](api-multipart-list-uploads.md)
+ [List Parts \(GET uploadID\)](api-multipart-list-parts.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)