# Get Vault Lock \(GET lock\-policy\)<a name="api-GetVaultLock"></a>

## Description<a name="api-GetVaultLock-description"></a>

This operation retrieves the following attributes from the `lock-policy` subresource set on the specified vault: 
+ The vault lock policy set on the vault\.
+ The state of the vault lock, which is either `InProgess` or `Locked`\.
+ When the lock ID expires\. The lock ID is used to complete the vault locking process\.
+ When the vault lock was initiated and put into the `InProgress` state\.

A vault lock is put into the `InProgress` state by calling [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)\. A vault lock is put into the `Locked` state by calling [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)\. You can stop the vault locking process by calling [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)\. For more information about the vault locking process, see [Amazon S3 Glacier Vault Lock](vault-lock.md)\.

If there is no vault lock policy set on the vault, the operation returns a `404 Not found` error\. For more information about vault lock policies, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.

## Requests<a name="api-GetVaultLock-requests"></a>

To return the current vault lock policy and other attributes, send an HTTP `GET` request to the URI of the vault's `lock-policy` subresource as shown in the following syntax example\.

### Syntax<a name="api-GetVaultLock-requests-syntax"></a>

```
1. GET /AccountId/vaults/vaultName/lock-policy HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-GetVaultLock-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-GetVaultLock-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-GetVaultLock-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-GetVaultLock-responses"></a>

In response, Amazon S3 Glacier \(S3 Glacier\) returns the vault access policy in JSON format in the body of the response\. 

### Syntax<a name="api-GetVaultLock-responses-syntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: length
				
{
  "Policy": "string",
  "State": "string",
  "ExpirationDate": "string",
  "CreationDate":"string"
}
```

### Response Headers<a name="api-GetVaultLock-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-GetVaultLock-responses-elements"></a>

The response body contains the following JSON fields\.

 **Policy**   
The vault lock policy as a JSON string, which uses "\\" as an escape character\.  
 Type: String

 **State**   
The state of the vault lock\.  
 Type: String  
 Valid values: `InProgress``|Locked`

 **ExpirationDate**   
The UTC date and time at which the lock ID expires\. This value can be `null` if the vault lock is in a `Locked` state\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

 **CreationDate**   
The UTC date and time at which the vault lock was put into the `InProgress` state\.  
*Type*: A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

### Errors<a name="api-GetVaultLock-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-GetVaultLock-examples"></a>

The following example demonstrates how to get a vault lock policy\.

### Example Request<a name="api-GetVaultLock-example-request"></a>

In this example, a `GET` request is sent to the URI of a vault's `lock-policy` subresource\.

```
1. GET /-/vaults/examplevault/lock-policy HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-GetVaultLock-example-response"></a>

If the request was successful, S3 Glacier returns the vault access policy as a JSON string in the body of the response\. The returned JSON string uses "\\" as an escape character, as shown in the [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) example request\. However, the following example shows the returned JSON string without escape characters for readability\. 

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: length
 6. 
 7. {
 8.   "Policy": "
 9.     {
10.       "Version": "2012-10-17",
11.       "Statement": [
12.         {
13.           "Sid": "Define-vault-lock",
14.           "Principal": {
15.             "AWS": "arn:aws:iam::999999999999:root"
16.           },
17.           "Effect": "Deny",
18.           "Action": "glacier:DeleteArchive",
19.           "Resource": [
20.             "arn:aws:glacier:us-west-2:999999999999:vaults/examplevault"
21.           ],
22.           "Condition": {
23.             "NumericLessThanEquals": {
24.               "glacier:ArchiveAgeInDays": "365"
25.             }
26.           }
27.         }
28.       ]
29.     }
30.   ",
31.   "State": "InProgress",
32.   "ExpirationDate": "exampledate",
33.   "CreationDate": "exampledate"  
34. }
```

## Related Sections<a name="related-sections-GetVaultLock"></a>
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)

## See Also<a name="api-GetVaultLock_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/get-vault-lock.html) 