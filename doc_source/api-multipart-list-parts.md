# List Parts \(GET uploadID\)<a name="api-multipart-list-parts"></a>

## Description<a name="api-multipart-list-partsDescription"></a>

This multipart upload operation lists the parts of an archive that have been uploaded in a specific multipart upload identified by an upload ID\. For information about multipart upload, see [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)\.

You can make this request at any time during an in\-progress multipart upload before you complete the multipart upload\. S3 Glacier returns the part list sorted by range you specified in each part upload\. If you send a List Parts request after completing the multipart upload, Amazon S3 Glacier \(S3 Glacier\) returns an error\. 

The List Parts operation supports pagination\. You should always check the `Marker` field in the response body for a marker at which to continue the list\. if there are no more items the `marker` field is `null`\. If the `marker` is not null, to fetch the next set of parts you sent another List Parts request with the `marker` request parameter set to the marker value S3 Glacier returned in response to your previous List Parts request\.

You can also limit the number of parts returned in the response by specifying the `limit` parameter in the request\. 

## Requests<a name="api-multipart-list-parts-requests"></a>

### Syntax<a name="api-multipart-list-parts-requests-syntax"></a>

To list the parts of an in\-progress multipart upload, you send a `GET` request to the URI of the multipart upload ID resource\. The multipart upload ID is returned when you initiate a multipart upload \([Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)\)\. You may optionally specify `marker` and `limit` parameters\.

```
1. GET /AccountId/vaults/VaultName/multipart-uploads/uploadID HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-multipart-list-parts-requests-parameters"></a>


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| limit  |  The maximum number of parts to be returned\. The default limit is 50\. The number of parts returned might be fewer than the specified limit, but the number of returned parts never exceeds the limit\. Type: String Constraints: Minimum integer value of `1`\. Maximum integer value of `50`\.  |  No  | 
|  marker  |  An opaque string used for pagination\. `marker` specifies the part at which the listing of parts should begin\. Get the `marker` value from the response of a previous List Parts response\. You need only include the `marker` if you are continuing the pagination of results started in a previous List Parts request\. Type: String Constraints: None  |  No | 

### Request Headers<a name="api-multipart-list-parts-requests-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Request Body<a name="api-multipart-list-parts-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-multipart-list-parts-responses"></a>

### Syntax<a name="api-multipart-list-parts-responses-syntax"></a>

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: x-amzn-RequestId
 3. Date: Date
 4. Content-Type: application/json
 5. Content-Length: Length
 6. 
 7. {
 8.     "ArchiveDescription" : String,
 9.     "CreationDate" : String,
10.     "Marker": String,
11.     "MultipartUploadId" : String,
12.     "PartSizeInBytes" : Number,
13.     "Parts" : 
14.     [ {
15.       "RangeInBytes" : String,
16.       "SHA256TreeHash" : String
17.       },
18.       ...
19.      ],
20.     "VaultARN" : String
21. }
```

### Response Headers<a name="api-multipart-list-parts-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-multipart-list-parts-responses-elements"></a>

The response body contains the following JSON fields\.

**ArchiveDescription**  
The description of the archive that was specified in the Initiate Multipart Upload request\. This field is `null` if no archive description was specified in the Initiate Multipart Upload operation\.  
*Type*: String

**CreationDate**  
The UTC time that the multipart upload was initiated\.  
*Type*: String\. A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**Marker**  
An opaque string that represents where to continue pagination of the results\. You use the `marker` in a new List Parts request to obtain more jobs in the list\. If there are no more parts, this value is `null`\.  
*Type*: String

**MultipartUploadId**  
The ID of the upload to which the parts are associated\.  
*Type*: String

**PartSizeInBytes**  
The part size in bytes\. This is the same value that you specified in the Initiate Multipart Upload request\.  
*Type*: Number

**Parts**  
A list of the part sizes of the multipart upload\. Each object in the array contains a `RangeBytes` and `sha256-tree-hash` name/value pair\.  
*Type*: Array

**RangeInBytes**  
The byte range of a part, inclusive of the upper value of the range\.  
*Type*: String

**SHA256TreeHash**   
The SHA256 tree hash value that S3 Glacier calculated for the part\. This field is never `null`\.  
*Type*: String

**VaultARN**  
The Amazon Resource Name \(ARN\) of the vault to which the multipart upload was initiated\.  
*Type*: String

### Errors<a name="api-multipart-list-parts-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-multipart-list-parts-examples"></a>

### Example: List Parts of a Multipart Upload<a name="api-multipart-list-parts-examples-one"></a>

The following example lists all the parts of an upload\. The example sends an HTTP `GET` request to the URI of the specific multipart upload ID of an in\-progress multipart upload and returns up to 1,000 parts\.

#### Example Request<a name="api-multipart-list-parts-example-request"></a>

```
1. GET /-/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-multipart-list-parts-example-response"></a>

In the response, S3 Glacier returns a list of uploaded parts associated with the specified multipart upload ID\. In this example, there are only two parts\. The returned `Marker` field is `null` indicating that there are no more parts of the multipart upload\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 412
 6.       
 7. {
 8.     "ArchiveDescription" : "archive description",
 9.     "CreationDate" : "2012-03-20T17:03:43.221Z",
10.     "Marker": null,
11.     "MultipartUploadId" : "OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE",
12.     "PartSizeInBytes" : 4194304,
13.     "Parts" : 
14.     [ {
15.       "RangeInBytes" : "0-4194303",
16.       "SHA256TreeHash" : "01d34dabf7be316472c93b1ef80721f5d4"
17.       },
18.       {
19.       "RangeInBytes" : "4194304-8388607",
20.       "SHA256TreeHash" : "0195875365afda349fc21c84c099987164"
21.       }],
22.     "VaultARN" : "arn:aws:glacier:us-west-2:012345678901:vaults/demo1-vault"
23. }
```

### Example: List Parts of a Multipart Upload \(Specify the Marker and the Limit Request Parameters\)<a name="api-multipart-list-parts-examples-two"></a>

The following example demonstrates how to use pagination to get a limited number of results\. The example sends an HTTP `GET` request to the URI of the specific multipart upload ID of an in\-progress multipart upload to return one part\. A starting `marker` parameter specifies at which part to start the part list\. You can get the `marker` value from the response of a previous request for a part list\. Furthermore, in this example, the `limit` parameter is set to 1 and returns one part\. Note that the `Marker` field is not `null`, indicating that there is at least one more part to obtain\. 

#### Example Request<a name="api-multipart-list-parts-example-request-two"></a>

```
1. GET /-/vaults/examplevault/multipart-uploads/OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE?marker=1001&limit=1 HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-multipart-list-parts-example-response-two"></a>

In the response, S3 Glacier returns a list of uploaded parts that are associated with the specified in\-progress multipart upload ID\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: text/json
 5. Content-Length: 412
 6.       
 7. {
 8.     "ArchiveDescription" : "archive description 1",
 9.     "CreationDate" : "2012-03-20T17:03:43.221Z",
10.     "Marker": "MfgsKHVjbQ6EldVl72bn3_n5h2TaGZQUO-Qb3B9j3TITf7WajQ",
11.     "MultipartUploadId" : "OW2fM5iVylEpFEMM9_HpKowRapC3vn5sSL39_396UW9zLFUWVrnRHaPjUJddQ5OxSHVXjYtrN47NBZ-khxOjyEXAMPLE",
12.     "PartSizeInBytes" : 4194304,
13.     "Parts" : 
14.     [ {
15.       "RangeInBytes" : "4194304-8388607",
16.       "SHA256TreeHash" : "01d34dabf7be316472c93b1ef80721f5d4"
17.       }],
18.     "VaultARN" : "arn:aws:glacier:us-west-2:012345678901:vaults/demo1-vault"
19. }
```

## Related Sections<a name="related-sections-api-multipart-list-parts"></a>
+ [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)
+ [Upload Part \(PUT uploadID\)](api-upload-part.md)
+ [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)
+ [Abort Multipart Upload \(DELETE uploadID\)](api-multipart-abort-upload.md)
+ [List Multipart Uploads \(GET multipart\-uploads\)](api-multipart-list-uploads.md)
+ [Uploading Large Archives in Parts \(Multipart Upload\)](uploading-archive-mpu.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)