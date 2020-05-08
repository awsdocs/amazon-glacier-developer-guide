# Describe Vault \(GET vault\)<a name="api-vault-get"></a>

## Description<a name="api-vault-get-description"></a>

This operation returns information about a vault, including the vault Amazon Resource Name \(ARN\), the date the vault was created, the number of archives contained within the vault, and the total size of all the archives in the vault\. The number of archives and their total size are as of the last vault inventory Amazon S3 Glacier \(S3 Glacier\) generated \(see [Working with Vaults in Amazon S3 Glacier](working-with-vaults.md)\)\. S3 Glacier generates vault inventories approximately daily\. This means that if you add or remove an archive from a vault, and then immediately send a Describe Vault request, the response might not reflect the changes\. 

## Requests<a name="api-vault-get-requests"></a>

To get information about a vault, send a `GET` request to the URI of the specific vault resource\.

### Syntax<a name="api-vault-get-requests-syntax"></a>

```
1. GET /AccountId/vaults/VaultName HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vault-get-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-vault-get-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vault-get-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-vault-get-responses"></a>

### Syntax<a name="api-vault-get-responses-syntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: Length

{
  "CreationDate" : String,
  "LastInventoryDate" : String,
  "NumberOfArchives" : Number,
  "SizeInBytes" : Number,
  "VaultARN" : String,
  "VaultName" : String
}
```

### Response Headers<a name="api-vault-get-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-vault-get-responses-elements"></a>

The response body contains the following JSON fields\.

**CreationDate**  
The UTC date when the vault was created\.   
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**LastInventoryDate**  
The UTC date when S3 Glacier completed the last vault inventory\. For information about initiating an inventory for a vault, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**NumberOfArchives**  
The number of archives in the vault as per the last vault inventory\. This field will return null if an inventory has not yet run on the vault, for example, if you just created the vault\.  
*Type*: Number

**SizeInBytes**  
The total size in bytes of the archives in the vault including any per\-archive overhead, as of the last inventory date\. This field will return `null` if an inventory has not yet run on the vault, for example, if you just created the vault\.  
*Type*: Number

**VaultARN**  
The Amazon Resource Name \(ARN\) of the vault\.  
*Type*: String

**VaultName**  
The vault name that was specified at creation time\. The vault name is also included in the vault's ARN\.  
*Type*: String

### Errors<a name="api-vault-get-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vault-get-examples"></a>

### Example Request<a name="api-vault-get-example-request"></a>

The following example demonstrates how to get information about the vault named `examplevault`\.

```
GET /-/vaults/examplevault HTTP/1.1
Host: glacier.us-west-2.amazonaws.com
x-amz-Date: 20170210T120000Z
x-amz-glacier-version: 2012-06-01
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-vault-get-example-response"></a>

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:02:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 260
 6. 
 7. {
 8.   "CreationDate" : "2012-02-20T17:01:45.198Z",
 9.   "LastInventoryDate" : "2012-03-20T17:03:43.221Z",
10.   "NumberOfArchives" : 192,
11.   "SizeInBytes" : 78088912,
12.   "VaultARN" : "arn:aws:glacier:us-west-2:012345678901:vaults/examplevault",
13.   "VaultName" : "examplevault"
14. }
```

## Related Sections<a name="related-sections-vault-get"></a>
+ [Create Vault \(PUT vault\)](api-vault-put.md)
+ [List Vaults \(GET vaults\)](api-vaults-get.md)
+ [Delete Vault \(DELETE vault\)](api-vault-delete.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="api-vault-get_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/describe-vault.html) 