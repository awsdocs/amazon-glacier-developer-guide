# Upload Archive \(POST archive\)<a name="api-archive-post"></a>

## Description<a name="api-archive-post-description"></a>

This operation adds an archive to a vault\. For a successful upload, your data is durably persisted\. In response, Amazon S3 Glacier \(S3 Glacier\) returns the archive ID in the `x-amz-archive-id` header of the response\. You should save the archive ID returned so that you can access the archive later\. 

You must provide a SHA256 tree hash of the data you are uploading\. For information about computing a SHA256 tree hash, see [Computing Checksums](checksum-calculations.md)\. 

When uploading an archive, you can optionally specify an archive description of up to 1,024 printable ASCII characters\. S3 Glacier returns the archive description when you either retrieve the archive or get the vault inventory\. S3 Glacier does not interpret the description in any way\. An archive description does not need to be unique\. You cannot use the description to retrieve or sort the archive list\. 

Except for the optional archive description, S3 Glacier does not support any additional metadata for the archives\. The archive ID is an opaque sequence of characters from which you cannot infer any meaning about the archive\. So you might maintain metadata about the archives on the client\-side\. For more information, see [Working with Archives in Amazon S3 Glacier](working-with-archives.md)\.

Archives are immutable\. After you upload an archive, you cannot edit the archive or its description\. 

## Requests<a name="api-archive-post-requests"></a>

To upload an archive, you use the HTTP `POST` method and scope the request to the `archives` subresource of the vault in which you want to save the archive\. The request must include the archive payload size, checksum \(SHA256 tree hash\), and can optionally include a description of the archive\.

### Syntax<a name="api-archive-post-requests-syntax"></a>

```
 1. POST /AccountId/vaults/VaultName/archives
 2. Host: glacier.Region.amazonaws.com
 3. x-amz-glacier-version: 2012-06-01
 4. Date: Date
 5. Authorization: SignatureValue
 6. x-amz-archive-description: Description
 7. x-amz-sha256-tree-hash: SHA256 tree hash
 8. x-amz-content-sha256: SHA256 linear hash
 9. Content-Length: Length
10. 
11. <Request body.>
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-archive-post-requests-parameters"></a>

This implementation of the operation does not use request parameters\.

### Request Headers<a name="api-archive-post-requests-headers"></a>

This operation uses the following request headers, in addition to the request headers that are common to all operations\. For more information about the common request headers, see [Common Request Headers](api-common-request-headers.md)\.


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| Content\-Length  |  The size of the object, in bytes\. For more information, go to [http://www\.w3\.org/Protocols/rfc2616/rfc2616\-sec14\.html\#sec14\.13](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.13)\.  Type: Number Default: None Constraints: None  |  Yes  | 
| x\-amz\-archive\-description  |  The optional description of the archive you are uploading\. It can be a plain language description or some identifier you choose to assign\. The description need not be unique across archives\. When you retrieve a vault inventory \(see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\), it includes this description for each of the archives it returns in response\. Type: String Default: None Constraints: The description must be less than or equal to 1,024 characters\. The allowable characters are 7\-bit ASCII without control codes, specifically ASCII values 32—126 decimal or 0x20—0x7E hexadecimal\.  |  No  | 
| x\-amz\-content\-sha256  |  The SHA256 checksum \(a linear hash\) of the payload\. This is not the same value as you specify in the `x-amz-sha256-tree-hash` header\. Type: String Default: None Constraints: None  |  Yes  | 
|  x\-amz\-sha256\-tree\-hash  |  The user\-computed checksum, SHA256 tree hash, of the payload\. For information on computing the SHA256 tree hash, see [Computing Checksums](checksum-calculations.md)\. If S3 Glacier computes a different checksum of the payload, it will reject the request\. Type: String Default: None Constraints: None  |  Yes | 

### Request Body<a name="api-archive-post-requests-elements"></a>

The request body contains the data to upload\.

## Responses<a name="api-archive-post-responses"></a>

 In response, S3 Glacier durably stores the archive and returns a URI path to the archive ID\.

### Syntax<a name="api-archive-post-response-syntax"></a>

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: x-amzn-RequestId
3. Date: Date
4. x-amz-sha256-tree-hash: ChecksumComputedByAmazonGlacier
5. Location: Location
6. x-amz-archive-id: ArchiveId
```

### Response Headers<a name="api-archive-post-responses-headers"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description  | 
| --- | --- | 
|  Location  |  The relative URI path of the newly added archive resource\.  Type: String  | 
|  x\-amz\-archive\-id  |  The ID of the archive\. This value is also included as part of the `Location` header\. Type: String  | 
|  x\-amz\-sha256\-tree\-hash​  |  The checksum of the archive computed by S3 Glacier\.  Type: String  | 

### Response Body<a name="api-archive-post-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-archive-post-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-archive-post-examples"></a>

### Example Request<a name="api-archive-post-examples-request"></a>

The following example shows a request to upload an archive\. 

```
 1. POST /-/vaults/examplevault/archives HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-sha256-tree-hash: beb0fe31a1c7ca8c6c04d574ea906e3f97b31fdca7571defb5b44dca89b5af60
 5. x-amz-content-sha256: 7f2fe580edb35154041fa3d4b41dd6d3adaef0c85d2ff6309f1d4b520eeecda3
 6. Content-Length: 2097152
 7. x-amz-glacier-version: 2012-06-01
 8. Authorization: Authorization=AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-glacier-version,Signature=16b9a9e220a37e32f2e7be196b4ebb87120ca7974038210199ac5982e792cace
 9. 
10. <Request body (2097152 bytes).>
```

### Example Response<a name="api-archive-post-examples-response"></a>

The successful response below has a `Location` header where you can get the ID that S3 Glacier assigned to the archive\.

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
4. x-amz-sha256-tree-hash: beb0fe31a1c7ca8c6c04d574ea906e3f97b31fdca7571defb5b44dca89b5af60
5. Location: /111122223333/vaults/examplevault/archives/NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId
6. x-amz-archive-id: NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId
```

## Related Sections<a name="related-sections-api-archive-post"></a>
+ [Working with Archives in Amazon S3 Glacier](working-with-archives.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Delete Archive \(DELETE archive\)](api-archive-delete.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)