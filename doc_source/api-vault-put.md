# Create Vault \(PUT vault\)<a name="api-vault-put"></a>

## Description<a name="api-vault-put-description"></a>

This operation creates a new vault with the specified name\.  The name of the vault must be unique within an AWS Region for an AWS account\. You can create up to 1,000 vaults per account\. For information on creating more vaults, go to the [Amazon S3 Glacier product detail page](http://aws.amazon.com/glacier)\.

You must use the following guidelines when naming a vault\. 
+  Names can be between 1 and 255 characters long\. 
+ Allowed characters are a–z, A–Z, 0–9, '\_' \(underscore\), '\-' \(hyphen\), and '\.' \(period\)\.

This operation is idempotent, you can send the same request multiple times and it has no further effect after the first time Amazon S3 Glacier \(S3 Glacier\) creates the specified vault\.

## Requests<a name="api-vault-put-requests"></a>

### Syntax<a name="api-vault-put-requests-syntax"></a>

To create a vault, send an HTTP PUT request to the URI of the vault to be created\.

```
1. PUT /AccountId/vaults/VaultName HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. Content-Length: Length
6. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vault-put-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-vault-put-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vault-put-requests-elements"></a>

The request body for this operation must be empty \(0 bytes\)\.

## Responses<a name="api-vault-put-responses"></a>

### Syntax<a name="api-vault-putresponse-syntax"></a>

```
HTTP/1.1 201 Created
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Location: Location
```

### Response Headers<a name="api-vault-put-responses-headers"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description | 
| --- | --- | 
| `Location`  | The relative URI path of the vault that was created\. Type: String | 

### Response Body<a name="api-vault-put-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-vault-put-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vault-put-examples"></a>

### Example Request<a name="api-vault-put-example-request"></a>

The following example sends an HTTP PUT request to create a vault named `examplevault`\. 

```
1. PUT /-/vaults/examplevault HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Content-Length: 0
6. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-vault-put-example-response"></a>

S3 Glacier creates the vault and returns the relative URI path of the vault in the `Location` header\. The account ID is always displayed in the `Location` header regardless of whether the account ID or a hyphen \('`-`'\) was specified in the request\.

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
4. Location: /111122223333/vaults/examplevault
```

## Related Sections<a name="related-sections-vault-put"></a>
+ [List Vaults \(GET vaults\)](api-vaults-get.md)
+ [Delete Vault \(DELETE vault\)](api-vault-delete.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="vault-put-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/create-vault.html) 