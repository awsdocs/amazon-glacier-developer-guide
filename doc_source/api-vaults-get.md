# List Vaults \(GET vaults\)<a name="api-vaults-get"></a>

## Description<a name="api-vaults-get-description"></a>

This operation lists all vaults owned by the calling user’s account\. The list returned in the response is ASCII\-sorted by vault name\. 

By default, this operation returns up to 10 items per request\. If there are more vaults to list, the `marker` field in the response body contains the vault Amazon Resource Name \(ARN\) at which to continue the list with a new List Vaults request; otherwise, the `marker` field is `null`\. In your next List Vaults request you set the `marker` parameter to the value Amazon S3 Glacier \(S3 Glacier\) returned in the responses to your previous List Vaults request\. You can also limit the number of vaults returned in the response by specifying the `limit` parameter in the request\. 

## Requests<a name="api-vaults-get-requests"></a>

To get a list of vaults, you send a `GET` request to the *vaults* resource\.

### Syntax<a name="api-vaults-get-requests-syntax"></a>

```
1. GET /AccountId/vaults HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vaults-get-requests-parameters"></a>

This operation uses the following request parameters\.


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  limit  |  The maximum number of vaults to be returned\. The default limit is 10\. The number of vaults returned might be fewer than the specified limit, but the number of returned vaults never exceeds the limit\. Type: String Constraints: Minimum integer value of 1\. Maximum integer value of 10\.  |  No  | 
|  marker  |  A string used for pagination\. `marker` specifies the vault ARN after which the listing of vaults should begin\. \(The vault specified by `marker` is not included in the returned list\.\) Get the `marker` value from a previous List Vaults response\. You need to include the `marker` only if you are continuing the pagination of results started in a previous List Vaults request\. Specifying an empty value \(""\) for the marker returns a list of vaults starting from the first vault\. Type: String Constraints: None  |  No  | 

### Request Headers<a name="api-vaults-get-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vaults-get-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-vaults-get-responses"></a>

### Syntax<a name="api-vaults-get-responses-syntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: Length

{
  "Marker": String
  "VaultList": [ 
   {
    "CreationDate": String,
    "LastInventoryDate": String,
    "NumberOfArchives": Number,
    "SizeInBytes": Number,
    "VaultARN": String,
    "VaultName": String
   }, 
   ...
  ]
}
```

### Response Headers<a name="api-vaults-get-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-vaults-get-response-body"></a>

The response body contains the following JSON fields\.

**CreationDate**  
The date the vault was created, in Coordinated Universal Time \(UTC\)\.  
*Type*: String\. A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**LastInventoryDate**  
The date of the last vault inventory, in Coordinated Universal Time \(UTC\)\. This field can be null if an inventory has not yet run on the vault, for example, if you just created the vault\. For information about initiating an inventory for a vault, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**Marker**  
The `vaultARN` that represents where to continue pagination of the results\. You use the `marker` in another List Vaults request to obtain more vaults in the list\. If there are no more vaults, this value is `null`\.   
*Type*: String

**NumberOfArchives**  
The number of archives in the vault as of the last inventory date\.  
*Type*: Number

**SizeInBytes**  
The total size, in bytes, of all the archives in the vault including any per\-archive overhead, as of the last inventory date\.  
*Type*: Number

**VaultARN**  
The Amazon Resource Name \(ARN\) of the vault\.  
*Type*: String

**VaultList**  
An array of objects, with each object providing a description of a vault\.  
*Type*: Array

**VaultName**  
The vault name\.  
*Type*: String

### Errors<a name="api-vaults-get-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vaults-get-examples"></a>

### Example: List All Vaults<a name="api-vaults-get-example1"></a>

The following example lists vaults\. Because the `marker` and `limit` parameters are not specified in the request, up to 10 vaults are returned\.

#### Example Request<a name="api-vaults-get-example1-request"></a>

```
1. GET /-/vaults HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-vaults-get-example1-response"></a>

The `Marker` is `null` indicating there are no more vaults to list\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:02:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 497	
 6. 
 7. {
 8.   "Marker": null,
 9.   "VaultList": [ 
10.    {
11.     "CreationDate": "2012-03-16T22:22:47.214Z",
12.     "LastInventoryDate": "2012-03-21T22:06:51.218Z",
13.     "NumberOfArchives": 2,
14.     "SizeInBytes": 12334,
15.     "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault1",
16.     "VaultName": "examplevault1"
17.    }, 
18.    {
19.     "CreationDate": "2012-03-19T22:06:51.218Z",
20.     "LastInventoryDate": "2012-03-21T22:06:51.218Z",
21.     "NumberOfArchives": 0,
22.     "SizeInBytes": 0,
23.     "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault2",
24.     "VaultName": "examplevault2"
25.    },
26.    {
27.     "CreationDate": "2012-03-19T22:06:51.218Z",
28.     "LastInventoryDate": "2012-03-25T12:14:31.121Z",
29.     "NumberOfArchives": 0,
30.     "SizeInBytes": 0,
31.     "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault3",
32.     "VaultName": "examplevault3"
33.    } 
34.   ]
35. }
```

### Example: Partial List of Vaults<a name="api-vaults-get-example2"></a>

The following example returns two vaults starting at the vault specified by the `marker`\.

#### Example Request<a name="api-vaults-get-example2-request"></a>

```
1. GET /-/vaults?limit=2&marker=arn:aws:glacier:us-west-2:012345678901:vaults/examplevault1 HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-vaults-get-example2-response"></a>

Two vaults are returned in the list\. The `Marker` contains the vault ARN to continue pagination in another List Vaults request\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:02:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 497	
 6. 
 7. {
 8.   "Marker": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault3",
 9.   "VaultList": [ 
10.    {
11.     "CreationDate": "2012-03-16T22:22:47.214Z",
12.     "LastInventoryDate": "2012-03-21T22:06:51.218Z",
13.     "NumberOfArchives": 2,
14.     "SizeInBytes": 12334,
15.     "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault1",
16.     "VaultName": "examplevault1"
17.    }, 
18.    {
19.     "CreationDate": "2012-03-19T22:06:51.218Z",
20.     "LastInventoryDate": "2012-03-21T22:06:51.218Z",
21.     "NumberOfArchives": 0,
22.     "SizeInBytes": 0,
23.     "VaultARN": "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault2",
24.     "VaultName": "examplevault2"
25.    }
26.   ]
27. }
```

## Related Sections<a name="related-sections-vaults-get"></a>
+ [Create Vault \(PUT vault\)](api-vault-put.md)
+ [Delete Vault \(DELETE vault\)](api-vault-delete.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="api-vaults-get_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/list-vaults.html) 