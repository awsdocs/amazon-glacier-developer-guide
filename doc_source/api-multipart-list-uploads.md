# List Multipart Uploads \(GET multipart\-uploads\)<a name="api-multipart-list-uploads"></a>

## Description<a name="api-multipart-list-uploads-description"></a>

This multipart upload operation lists in\-progress multipart uploads for the specified vault\. An in\-progress multipart upload is a multipart upload that has been initiated by an [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md) request, but has not yet been completed or stopped\. The list returned in the List Multipart Upload response has no guaranteed order\.

The List Multipart Uploads operation supports pagination\. By default, this operation returns up to 50 multipart uploads in the response\. You should always check the `marker` field in the response body for a marker at which to continue the list; if there are no more items the `marker` field is `null`\. 

If the `marker` is not null, to fetch the next set of multipart uploads you sent another List Multipart Uploads request with the `marker` request parameter set to the marker value Amazon S3 Glacier \(S3 Glacier\) returned in response to your previous List Multipart Uploads request\.

Note the difference between this operation and the [List Parts \(GET uploadID\)](api-multipart-list-parts.md)\) operation\. The List Multipart Uploads operation lists all multipart uploads for a vault\. The List Parts operation returns parts of a specific multipart upload identified by an Upload ID\.

For information about multipart upload, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

## Requests<a name="api-multipart-list-uploads-requests"></a>

### Syntax<a name="api-multipart-list-uploads-requests-syntax"></a>

To list multipart uploads, send a `GET` request to the URI of the `multipart-uploads` subresource of the vault\. You may optionally specify `marker` and `limit` parameters\.

```
1. GET /AccountId/vaults/VaultName/multipart-uploads HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-multipart-list-uploads-requests-parameters"></a>


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  limit  |  Specifies the maximum number of uploads returned in the response body\. If not specified, the List Uploads operation returns up to 50 uploads\. Type: String Constraints: Minimum integer value of `1`\. Maximum integer value of `50`\.  |  No | 
| marker  |  An opaque string used for pagination\. `marker` specifies the upload at which the listing of uploads should begin\. Get the `marker` value from a previous List Uploads response\. You need only include the `marker` if you are continuing the pagination of results started in a previous List Uploads request\. Type: String Constraints: None  |  No  | 

### Request Headers<a name="api-multipart-list-uploads-requests-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Request Body<a name="api-multipart-list-uploads-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-multipart-list-uploads-responses"></a>

### Syntax<a name="api-multipart-list-uploads-responses-syntax"></a>

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: x-amzn-RequestId
 3. Date: Date
 4. Content-Type: application/json
 5. Content-Length: Length
 6. 
 7. {
 8.   "Marker": String,
 9.   "UploadsList" : [ 
10.     {
11.       "ArchiveDescription": String,
12.       "CreationDate": String,
13.       "MultipartUploadId": String,
14.       "PartSizeInBytes": Number,
15.       "VaultARN": String
16.     }, 
17.    ...
18.   ]
19. }
```

### Response Headers<a name="api-multipart-list-uploads-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-multipart-list-uploads-responses-elements"></a>

The response body contains the following JSON fields\.

**ArchiveDescription**  
The description of the archive that was specified in the Initiate Multipart Upload request\. This field is `null` if no archive description was specified in the Initiate Multipart Upload operation\.  
*Type*: String

**CreationDate**  
The UTC time that the multipart upload was initiated\.  
*Type*: String\. A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**Marker**  
An opaque string that represents where to continue pagination of the results\. You use the `marker` in a new List Multipart Uploads request to obtain more uploads in the list\. If there are no more uploads, this value is `null`\.  
*Type*: String

**PartSizeInBytes**  
The part size specified in the [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md) request\. This is the size of all the parts in the upload except the last part, which may be smaller than this size\.  
*Type*: Number

**MultipartUploadId**  
The ID of the multipart upload\.  
*Type*: String

**UploadsList**  
A list of metadata about multipart upload objects\. Each item in the list contains a set of name\-value pairs for the corresponding upload, including `ArchiveDescription`, `CreationDate`, `MultipartUploadId`, `PartSizeInBytes`, and `VaultARN`\.  
*Type*: Array

**VaultARN**  
The Amazon Resource Name \(ARN\) of the vault that contains the archive\.  
*Type*: String

### Errors<a name="api-multipart-list-uploads-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-multipart-list-uploads-examples"></a>

### Example: List All Multipart Uploads<a name="api-multipart-list-uploads-examples-one"></a>

The following example lists all the multipart uploads in progress for the vault\. The example shows an HTTP `GET` request to the URI of the `multipart-uploads` subresource of a specified vault\. Because the `marker` and `limit` parameters are not specified in the request, up to 1,000 in\-progress multipart uploads are returned\.

#### Example Request<a name="api-multipart-list-uploads-example-request"></a>

```
1. GET /-/vaults/examplevault/multipart-uploads HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-multipart-list-uploads-example-response"></a>

In the response S3 Glacier returns a list of all in\-progress multipart uploads for the specified vault\. The `marker` field is `null`, which indicates that there are no more uploads to list\. 

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 1054
 6.       
 7. {
 8.   "Marker": null, 
 9.   "UploadsList": [ 
10.     {
11.       "ArchiveDescription": "archive 1",
12.       "CreationDate": "2012-03-19T23:20:59.130Z",
13.       "MultipartUploadId": "xsQdFIRsfJr20CW2AbZBKpRZAFTZSJIMtL2hYf8mvp8dM0m4RUzlaqoEye6g3h3ecqB_zqwB7zLDMeSWhwo65re4C4Ev",
14.       "PartSizeInBytes": 4194304,
15.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
16.     }, 
17.     {
18.       "ArchiveDescription": "archive 2",
19.       "CreationDate": "2012-04-01T15:00:00.000Z",
20.       "MultipartUploadId": "nPyGOnyFcx67qqX7E-0tSGiRi88hHMOwOxR-_jNyM6RjVMFfV29lFqZ3rNsSaWBugg6OP92pRtufeHdQH7ClIpSF6uJc",
21.       "PartSizeInBytes": 4194304,
22.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
23.     },
24.     {
25.       "ArchiveDescription": "archive 3",
26.       "CreationDate": "2012-03-20T17:03:43.221Z",
27.       "MultipartUploadId": "qt-RBst_7yO8gVIonIBsAxr2t-db0pE4s8MNeGjKjGdNpuU-cdSAcqG62guwV9r5jh5mLyFPzFEitTpNE7iQfHiu1XoV",
28.       "PartSizeInBytes": 4194304,
29.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
30.     } 
31.   ]
32. }
```

### Example: Partial List of Multipart Uploads<a name="api-multipart-list-uploads-examples-two"></a>

The following example demonstrates how to use pagination to get a limited number of results\. The example shows an HTTP `GET` request to the URI of the `multipart-uploads` subresource for a specified vault\. In this example, the `limit` parameter is set to 1, which means that only one upload is returned in the list, and the `marker` parameter indicates the multipart upload ID at which the returned list begins\.

#### Example Request<a name="api-multipart-list-uploads-example-request-two"></a>

```
1. GET /-/vaults/examplevault/multipart-uploads?limit=1&marker=xsQdFIRsfJr20CW2AbZBKpRZAFTZSJIMtL2hYf8mvp8dM0m4RUzlaqoEye6g3h3ecqB_zqwB7zLDMeSWhwo65re4C4Ev HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-multipart-list-uploads-example-response-two"></a>

In the response, Amazon S3 Glacier \(S3 Glacier\) returns a list of no more than two in\-progress multipart uploads for the specified vault, starting at the specified marker and returning two results\. 

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 470
 6. 
 7. {
 8.   "Marker": "qt-RBst_7yO8gVIonIBsAxr2t-db0pE4s8MNeGjKjGdNpuU-cdSAcqG62guwV9r5jh5mLyFPzFEitTpNE7iQfHiu1XoV",
 9.   "UploadsList" : [ 
10.     {
11.       "ArchiveDescription": "archive 2",
12.       "CreationDate": "2012-04-01T15:00:00.000Z",
13.       "MultipartUploadId": "nPyGOnyFcx67qqX7E-0tSGiRi88hHMOwOxR-_jNyM6RjVMFfV29lFqZ3rNsSaWBugg6OP92pRtufeHdQH7ClIpSF6uJc",
14.       "PartSizeInBytes": 4194304,
15.       "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault"
16.     }
17.   ]
18. }
```

## Related Sections<a name="related-sections-multipart-list-uploads"></a>
+ [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)
+ [Upload Part \(PUT uploadID\)](api-upload-part.md)
+ [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)
+ [Abort Multipart Upload \(DELETE uploadID\)](api-multipart-abort-upload.md)
+ [List Parts \(GET uploadID\)](api-multipart-list-parts.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)