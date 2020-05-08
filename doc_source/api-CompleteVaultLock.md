# Complete Vault Lock \(POST lockId\)<a name="api-CompleteVaultLock"></a>

## Description<a name="api-CompleteVaultLock-description"></a>

This operation completes the vault locking process by transitioning the vault lock from the `InProgress` state to the `Locked` state, which causes the vault lock policy to become unchangeable\. A vault lock is put into the `InProgress` state by calling [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)\. You can obtain the state of the vault lock by calling [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)\. For more information about the vault locking process, see [Amazon S3 Glacier Vault Lock](vault-lock.md)\. 

This operation is idempotent\. This request is always successful if the vault lock is in the `Locked` state and the provided lock ID matches the lock ID originally used to lock the vault\.

If an invalid lock ID is passed in the request when the vault lock is in the `Locked` state, the operation returns an `AccessDeniedException` error\. If an invalid lock ID is passed in the request when the vault lock is in the `InProgress` state, the operation throws an `InvalidParameter` error\.

## Requests<a name="api-CompleteVaultLock-requests"></a>

To complete the vault locking process, send an HTTP `POST` request to the URI of the vault's `lock-policy` subresource with a valid lock ID\.

### Syntax<a name="api-CompleteVaultLock-requests-syntax"></a>

```
1. POST /AccountId/vaults/vaultName/lock-policy/lockId HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. Content-Length: Length
6. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

 The `lockId` value is the lock ID obtained from a [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) request\.

### Request Parameters<a name="api-CompleteVaultLock-requestParameters"></a>

#### Request Headers<a name="api-CompleteVaultLock-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

#### Request Body<a name="api-CompleteVaultLock-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-CompleteVaultLock-responses"></a>

If the operation request is successful, the service returns an HTTP `204 No Content` response\.

### Syntax<a name="api-CompleteVaultLock-response-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-CompleteVaultLock-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-CompleteVaultLock-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-CompleteVaultLock-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-CompleteVaultLock-examples"></a>

### Example Request<a name="api-CompleteVaultLock-example-request"></a>

The following example sends an HTTP POST request with the lock ID to complete the vault locking process\. 

```
1. POST /-/vaults/examplevault/lock-policy/AE863rKkWZU53SLW5be4DUcW HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
5. Content-Length: length
6. x-amz-glacier-version: 2012-06-01
```

### Example Response<a name="api-CompleteVaultLock-example-response"></a>

If the request was successful, Amazon S3 Glacier \(S3 Glacier\) returns an `HTTP 204 No Content` response, as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
```

## Related Sections<a name="related-sections-CompleteVaultLock"></a>
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)

## See Also<a name="api-CompleteVaultLock-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/complete-vault-lock.html) 