# Initiate Vault Lock \(POST lock\-policy\)<a name="api-InitiateVaultLock"></a>

## Description<a name="api-InitiateVaultLock-description"></a>

This operation initiates the vault locking process by doing the following: 
+ Installing a vault lock policy on the specified vault\.
+ Setting the lock state of vault lock to `InProgress`\.
+ Returning a lock ID, which is used to complete the vault locking process\. 

You can set one vault lock policy for each vault and this policy can be up to 20 KB in size\. For more information about vault lock policies, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.

You must complete the vault locking process within 24 hours after the vault lock enters the `InProgress` state\. After the 24 hour window ends, the lock ID expires, the vault automatically exits the `InProgress` state, and the vault lock policy is removed from the vault\. You call [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md) to complete the vault locking process by setting the state of the vault lock to `Locked`\. 

**Note**  
After a vault lock is in the `Locked` state, you cannot initiate a new vault lock for the vault\.

You can stop the vault locking process by calling [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)\. You can get the state of the vault lock by calling [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)\. For more information about the vault locking process, see [Amazon S3 Glacier Vault Lock](vault-lock.md)\.

If this operation is called when the vault lock is in the `InProgress` state, the operation returns an `AccessDeniedException` error\. When the vault lock is in the `InProgress` state you must call [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md) before you can initiate a new vault lock policy\. 

## Requests<a name="api-InitiateVaultLock-requests"></a>

To initiate the vault locking process, send an HTTP `POST` request to the URI of the `lock-policy` subresource of the vault, as shown in the following syntax example\.

### Syntax<a name="api-InitiateVaultLock-requests-syntax"></a>

```
 1. POST /AccountId/vaults/vaultName/lock-policy HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. Content-Length: Length
 6. x-amz-glacier-version: 2012-06-01
 7. 			
 8. {
 9.   "Policy": "string"
10. }
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-InitiateVaultLock-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-InitiateVaultLock-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-InitiateVaultLock-requests-elements"></a>

The request body contains the following JSON fields\.

 **Policy**   
The vault lock policy as a JSON string, which uses "\\" as an escape character\.  
 Type: String   
 Required: Yes

## Responses<a name="api-InitiateVaultLock-responses"></a>

Amazon S3 Glacier \(S3 Glacier\) returns an `HTTP 201 Created` response, if the policy is accepted\.

### Syntax<a name="api-InitiateVaultLock-response-syntax"></a>

```
HTTP/1.1 201 Created
x-amzn-RequestId: x-amzn-RequestId
Date: Date
x-amz-lock-id: lockId
```

### Response Headers<a name="api-InitiateVaultLock-responses-headers"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description  | 
| --- | --- | 
|  x\-amz\-lock\-id  |  The lock ID, which is used to complete the vault locking process\.  Type: String  | 

### Response Body<a name="api-InitiateVaultLock-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-InitiateVaultLock-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-InitiateVaultLock-examples"></a>

### Example Request<a name="api-InitiateVaultLock-example-request"></a>

The following example sends an HTTP `PUT` request to the URI of the vault's `lock-policy` subresource\. The `Policy` JSON string uses "\\" as an escape character\.

```
1. PUT /-/vaults/examplevault/lock-policy HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
5. Content-Length: length
6. x-amz-glacier-version: 2012-06-01
7. 
8. {"Policy":"{\"Version\":\"2012-10-17\",\"Statement\":[{\"Sid\":\"Define-vault-lock\",\"Effect\":\"Deny\",\"Principal\":{\"AWS\":\"arn:aws:iam::999999999999:root\"},\"Action\":\"glacier:DeleteArchive\",\"Resource\":\"arn:aws:glacier:us-west-2:999999999999:vaults/examplevault\",\"Condition\":{\"NumericLessThanEquals\":{\"glacier:ArchiveAgeinDays\":\"365\"}}}]}"}
```

### Example Response<a name="api-InitiateVaultLock-example-response"></a>

If the request was successful, S3 Glacier returns an `HTTP 201 Created` response, as shown in the following example\.

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
4. x-amz-lock-id: AE863rKkWZU53SLW5be4DUcW
```

## Related Sections<a name="related-sections-InitiateVaultLock"></a>
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)

## See Also<a name="api-InitiateVaultLock_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/initiate-vault-lock.html) 