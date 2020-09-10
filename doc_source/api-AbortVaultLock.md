# Abort Vault Lock \(DELETE lock\-policy\)<a name="api-AbortVaultLock"></a>

## Description<a name="api-AbortVaultLock-description"></a>

This operation stops the vault locking process if the vault lock is not in the `Locked` state\. If the vault lock is in the `Locked` state when this operation is requested, the operation returns an `AccessDeniedException` error\. Stopping the vault locking process removes the vault lock policy from the specified vault\. 

A vault lock is put into the `InProgress` state by calling [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)\. A vault lock is put into the `Locked` state by calling [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)\. You can get the state of a vault lock by calling [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)\. For more information about the vault locking process, see [Amazon S3 Glacier Vault Lock](vault-lock.md)\. For more information about vault lock policies, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.

This operation is idempotent\. You can successfully invoke this operation multiple times, if the vault lock is in the `InProgress` state or if there is no policy associated with the vault\.

## Requests<a name="api-AbortVaultLock-requests"></a>

To delete the vault lock policy, send an HTTP `DELETE` request to the URI of the vault's `lock-policy` subresource\.

### Syntax<a name="api-AbortVaultLock-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/vaultName/lock-policy HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-AbortVaultLock-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-AbortVaultLock-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-AbortVaultLock-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-AbortVaultLock-responses"></a>

If the policy is successfully deleted, S3 Glacier returns an `HTTP 204 No Content` response\.

### Syntax<a name="api-AbortVaultLock-responses-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-AbortVaultLock-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-AbortVaultLock-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-AbortVaultLock-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-AbortVaultLock-examples"></a>

The following example demonstrates how to stop the vault locking process\.

### Example Request<a name="api-AbortVaultLock-example-request"></a>

In this example, a `DELETE` request is sent to the `lock-policy` subresource of the vault named **examplevault**\.

```
1. DELETE /-/vaults/examplevault/lock-policy HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
5. x-amz-glacier-version: 2012-06-01
```

### Example Response<a name="api-AbortVaultLock-example-response"></a>

If the policy is successfully deleted S3 Glacier returns an `HTTP 204 No Content` response, as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-AbortVaultLock"></a>
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)

## See Also<a name="api-AbortVaultLock-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/abort-vault-lock.html) 