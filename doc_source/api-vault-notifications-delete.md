# Delete Vault Notifications \(DELETE notification\-configuration\)<a name="api-vault-notifications-delete"></a>

## Description<a name="api-vault-notifications-delete-description"></a>

This operation deletes the notification configuration set for a vault [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)\. The operation is eventually consistent—that is, it might take some time for Amazon S3 Glacier \(S3 Glacier\) to completely disable the notifications, and you might still receive some notifications for a short time after you send the delete request\. 

## Requests<a name="api-vault-notifications-delete-requests"></a>

To delete a vault's notification configuration, send a `DELETE` request to the vault's `notification-configuration` subresource\.

### Syntax<a name="api-vault-notifications-delete-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/VaultName/notification-configuration HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vault-notifications-delete-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-vault-notifications-delete-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vault-notifications-delete-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-vault-notifications-delete-responses"></a>

### Syntax<a name="api-vault-notifications-delete-responses-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-vault-notifications-delete-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-vault-notifications-delete-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-vault-notifications-delete-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vault-notifications-delete-examples"></a>

The following example demonstrates how to remove notification configuration for a vault\.

### Example Request<a name="api-vault-notifications-delete-example-request"></a>

In this example, a `DELETE` request is sent to the `notification-configuration` subresource of the vault called `examplevault`\.

```
1. DELETE /111122223333/vaults/examplevault/notification-configuration HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z 
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-vault-notifications-delete-example-response"></a>

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-vault-notifications-delete"></a>
+ [Get Vault Notifications \(GET notification\-configuration\)](api-vault-notifications-get.md)
+ [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="api-vault-notifications-delete_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/delete-vault-notifications.html) 