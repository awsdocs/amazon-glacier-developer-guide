# Delete Vault Access Policy \(DELETE access\-policy\)<a name="api-DeleteVaultAccessPolicy"></a>

## Description<a name="api-DeleteVaultAccessPolicy-description"></a>

This operation deletes the access policy associated with the specified vault\. The operation is eventually consistent—that is, it might take some time for Amazon S3 Glacier \(S3 Glacier\) to completely remove the access policy, and you might still see the effect of the policy for a short time after you send the delete request\. 

This operation is idempotent\. You can invoke delete multiple times, even if there is no policy associated with the vault\. For more information about vault access policies, see [Amazon S3 Glacier Access Control with Vault Access Policies](vault-access-policy.md)\.

## Requests<a name="api-DeleteVaultAccessPolicy-requests"></a>

To delete the current vault access policy, send an HTTP `DELETE` request to the URI of the vault's `access-policy` subresource\.

### Syntax<a name="api-DeleteVaultAccessPolicy-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/vaultName/access-policy HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-DeleteVaultAccessPolicy-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-DeleteVaultAccessPolicy-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-DeleteVaultAccessPolicy-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-DeleteVaultAccessPolicy-responses"></a>

In response, S3 Glacier returns `204 No Content` if the policy is successfully deleted\.

### Syntax<a name="api-DeleteVaultAccessPolicy-responses-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-DeleteVaultAccessPolicy-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-DeleteVaultAccessPolicy-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-DeleteVaultAccessPolicy-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-DeleteVaultAccessPolicy-examples"></a>

The following example demonstrates how to delete a vault access policy\.

### Example Request<a name="api-DeleteVaultAccessPolicy-example-request"></a>

In this example, a `DELETE` request is sent to the `access-policy` subresource of the vault named **examplevault**\.

```
1. DELETE /-/vaults/examplevault/access-policy HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
5. x-amz-glacier-version: 2012-06-01
```

### Example Response<a name="api-DeleteVaultAccessPolicy-example-response"></a>

In response, if the policy is successfully deleted S3 Glacier returns a `204 No Content` as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-DeleteVaultAccessPolicy"></a>
+ [Get Vault Access Policy \(GET access\-policy\)](api-GetVaultAccessPolicy.md)
+ [Set Vault Access Policy \(PUT access\-policy\)](api-SetVaultAccessPolicy.md)

## See Also<a name="api-DeleteVaultAccessPolicy-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/delete-vault-access-policy.html) 