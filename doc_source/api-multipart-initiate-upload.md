# Initiate Multipart Upload \(POST multipart\-uploads\)<a name="api-multipart-initiate-upload"></a>

## Description<a name="api-multipart-initiate-upload-description"></a>

This operation initiates a multipart upload \(see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\)\. Amazon S3 Glacier \(S3 Glacier\) creates a multipart upload resource and returns its ID in the response\. You use this Upload ID in subsequent multipart upload operations\.

When you initiate a multipart upload, you specify the part size in number of bytes\. The part size must be a megabyte \(1024 KB\) multiplied by a power of 2—for example, 1048576 \(1 MB\), 2097152 \(2 MB\), 4194304 \(4 MB\), 8388608 \(8 MB\), and so on\. The minimum allowable part size is 1 MB, and the maximum is 4 GB\.

Every part you upload using this upload ID, except the last one, must have the same size\. The last one can be the same size or smaller\. For example, suppose you want to upload a 16\.2 MB file\. If you initiate the multipart upload with a part size of 4 MB, you will upload four parts of 4 MB each and one part of 0\.2 MB\. 

**Note**  
You don't need to know the size of the archive when you start a multipart upload because S3 Glacier does not require you to specify the overall archive size\.

After you complete the multipart upload, S3 Glacier removes the multipart upload resource referenced by the ID\. S3 Glacier will also remove the multipart upload resource if you cancel the multipart upload or it may be removed if there is no activity for a period of 24 hours\. The ID may still be available after 24 hours, but applications should not expect this behavior\.

## Requests<a name="api-multipart-initiate-upload-requests"></a>

To initiate a multipart upload, you send an HTTP `POST` request to the URI of the `multipart-uploads` subresource of the vault in which you want to save the archive\. The request must include the part size and can optionally include a description of the archive\.

### Syntax<a name="api-multipart-initiate-upload-requests-syntax"></a>

```
1. POST /AccountId/vaults/VaultName/multipart-uploads 
2. Host: glacier.us-west-2.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
6. x-amz-archive-description: ArchiveDescription
7. x-amz-part-size: PartSize
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-archive-post-requests-parameters1"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-archive-post-requests-headers1"></a>

This operation uses the following request headers, in addition to the request headers that are common to all operations\. For more information about the common request headers, see [Common Request Headers](api-common-request-headers.md)\.


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  x\-amz\-part\-size  |  The size of each part except the last, in bytes\. The last part can be smaller than this part size\. Type: String Default: None Constraints: The part size must be a megabyte \(1024 KB\) multiplied by a power of 2—for example, 1048576 \(1 MB\), 2097152 \(2 MB\), 4194304 \(4 MB\), 8388608 \(8 MB\), and so on\. The minimum allowable part size is 1 MB, and the maximum is 4 GB \(4096 MB\)\.  |  Yes | 
| x\-amz\-archive\-description  |  Archive description you are uploading in parts\. It can be a plain\-language description or some unique identifier you choose to assign\. When you retrieve a vault inventory \(see [Initiate Job \(POST jobs\)](api-initiate-job-post.md) \), the inventory includes this description for each of the archives it returns in response\. Leading whitespace in archive descriptions is removed\. Type: String Default: None Constraints: The description must be less than or equal to 1024 bytes\. The allowable characters are 7 bit ASCII without control codes, specifically ASCII values 32\-126 decimal or 0x20\-0x7E hexadecimal\.  |  No  | 

### Request Body<a name="api-multipart-initiate-upload-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-multipart-initiate-upload-responses"></a>

In the response, S3 Glacier creates a multipart upload resource identified by an ID and returns the relative URI path of the multipart upload ID\. 

### Syntax<a name="api-multipart-initiate-upload-response-syntax"></a>

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: x-amzn-RequestId
3. Date: Date
4. Location: Location
5. x-amz-multipart-upload-id: multiPartUploadId
```

### Response Headers<a name="api-archive-post-responses-headers2"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description  | 
| --- | --- | 
|  Location  |  The relative URI path of the multipart upload ID S3 Glacier created\. You use this URI path to scope your requests to upload parts, and to complete the multipart upload\.  Type: String  | 
|  x\-amz\-multipart\-upload\-id  |  The ID of the multipart upload\. This value is also included as part of the `Location` header\.  Type: String  | 

### Response Body<a name="api-archive-post-responses-elements1"></a>

This operation does not return a response body\.

### Errors<a name="api-archive-post-responses-errors1"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Example<a name="initiate-mpu-api-example"></a>

### Example Request<a name="api-multipart-initiate-upload-example-request"></a>

The following example initiates a multipart upload by sending an HTTP `POST` request to the URI of the `multipart-uploads` subresource of a vault named `examplevault`\. The request includes headers to specify the part size of 4 MB \(4194304 bytes\) and the optional archive description\.

```
1. POST /-/vaults/examplevault/multipart-uploads 
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-archive-description: MyArchive-101
5. x-amz-part-size: 4194304
6. x-amz-glacier-version: 2012-06-01
7. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-multipart-initiate-upload-example-response"></a>

S3 Glacier creates a multipart upload resource and adds it to the `multipart-uploads` subresource of the vault\. The `Location` response header includes the relative URI path to the multipart upload ID\. 

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. Location: /111122223333/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE
5. x-amz-multipart-upload-id: OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE
```

For information about uploading individual parts, see [Upload Part \(PUT uploadID\)](api-upload-part.md)\.

## Related Sections<a name="related-sections-initiate-mpu"></a>
+ [Upload Part \(PUT uploadID\)](api-upload-part.md)
+ [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)
+ [Abort Multipart Upload \(DELETE uploadID\)](api-multipart-abort-upload.md)
+ [List Multipart Uploads \(GET multipart\-uploads\)](api-multipart-list-uploads.md)
+ [List Parts \(GET uploadID\)](api-multipart-list-parts.md)
+ [Delete Archive \(DELETE archive\)](api-archive-delete.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)