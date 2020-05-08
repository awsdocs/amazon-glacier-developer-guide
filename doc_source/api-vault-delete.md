# Delete Vault \(DELETE vault\)<a name="api-vault-delete"></a>

## Description<a name="api-vault-delete-description"></a>

This operation deletes a vault\. Amazon S3 Glacier \(S3 Glacier\) will delete a vault only if there are no archives in the vault as per the last inventory and there have been no writes to the vault since the last inventory\. If either of these conditions is not satisfied, the vault deletion fails \(that is, the vault is not removed\) and S3 Glacier returns an error\. 

You can use the [Describe Vault \(GET vault\)](api-vault-get.md) operation that provides vault information, including the number of archives in the vault; however, the information is based on the vault inventory S3 Glacier last generated\.

This operation is idempotent\.

**Note**  
When you delete a vault, the vault access policy attached to the vault is also deleted\. For more information about vault access policies, see [Amazon S3 Glacier Access Control with Vault Access Policies](vault-access-policy.md)\.

## Requests<a name="api-vault-delete-requests"></a>

To delete a vault, send a `DELETE` request to the vault resource URI\.

### Syntax<a name="api-vault-delete-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/VaultName HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vault-delete-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-vault-delete-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vault-delete-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-vault-delete-responses"></a>

### Syntax<a name="api-vault-delete-response-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-vault-delete-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-vault-delete-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-vault-delete-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vault-delete-examples"></a>

### Example Request<a name="api-vault-delete-example-request"></a>

The following example deletes a vault named `examplevault`\. The example request is a `DELETE` request to the URI of the resource \(the vault\) to delete\. 

```
1. DELETE /-/vaults/examplevault HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-vault-delete-example-response"></a>

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
```

## Related Sections<a name="related-sections-vault-delete"></a>
+ [Create Vault \(PUT vault\)](api-vault-put.md)
+ [List Vaults \(GET vaults\)](api-vaults-get.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="api-vault-delete-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/delete-vault.html) 