# Abort Multipart Upload \(DELETE uploadID\)<a name="api-multipart-abort-upload"></a>

## Description<a name="api-multipart-abort-upload-description"></a>

This multipart upload operation stops a multipart upload identified by the upload ID\.

After the Abort Multipart Upload request succeeds, you cannot use the upload ID to upload any more parts or perform any other operations\. Stopping a completed multipart upload fails\. However, stopping an already\-stopped upload will succeed, for a short time\. 

This operation is idempotent\. 

For information about multipart upload, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

## Requests<a name="api-multipart-abort-upload-requests"></a>

To stop a multipart upload, send an HTTP `DELETE` request to the URI of the `multipart-uploads` subresource of the vault and identify the specific multipart upload ID as part of the URI\.

### Syntax<a name="api-multipart-abort-upload-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/VaultName/multipart-uploads/uploadID HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-multipart-abort-upload-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-multipart-abort-upload-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-multipart-abort-upload-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-multipart-abort-upload-responses"></a>

### Syntax<a name="api-multipart-abort-upload-responses-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-multipart-abort-upload-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-multipart-abort-upload-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-multipart-abort-upload-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Example<a name="api-multipart-abort-upload-examples"></a>

### Example Request<a name="api-multipart-abort-upload-example-request"></a>

In the following example, a `DELETE` request is sent to the URI of a multipart upload ID resource\.

```
1. DELETE /-/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE  HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-multipart-abort-upload-example-response"></a>

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-multipart-abort-upload"></a>
+ [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)
+ [Upload Part \(PUT uploadID\)](api-upload-part.md)
+ [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)
+ [List Multipart Uploads \(GET multipart\-uploads\)](api-multipart-list-uploads.md)
+ [List Parts \(GET uploadID\)](api-multipart-list-parts.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)