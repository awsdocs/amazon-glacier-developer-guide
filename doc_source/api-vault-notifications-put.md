# Set Vault Notification Configuration \(PUT notification\-configuration\)<a name="api-vault-notifications-put"></a>

## Description<a name="api-vault-notifications-put-description"></a>

Retrieving an archive and a vault inventory are asynchronous operations in Amazon S3 Glacier \(S3 Glacier\) for which you must first initiate a job and wait for the job to complete before you can download the job output\. You can configure a vault to post a message to an Amazon Simple Notification Service \(Amazon SNS\) topic when these jobs complete\. You can use this operation to set notification configuration on the vault\. For more information, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\.  

To configure vault notifications, send a PUT request to the `notification-configuration` subresource of the vault\. A notification configuration is specific to a vault; therefore, it is also referred to as a vault subresource\. The request should include a JSON document that provides an Amazon Simple Notification Service \(Amazon SNS\) topic and the events for which you want S3 Glacier to send notifications to the topic\.

You can configure a vault to publish a notification for the following vault events:
+ **`ArchiveRetrievalCompleted`—** This event occurs when a job that was initiated for an archive retrieval is completed \([Initiate Job \(POST jobs\)](api-initiate-job-post.md)\)\. The status of the completed job can be `Succeeded` or `Failed`\. The notification sent to the SNS topic is the same output as returned from [Describe Job \(GET JobID\)](api-describe-job-get.md)\.
+ **`InventoryRetrievalCompleted`—** This event occurs when a job that was initiated for an inventory retrieval is completed \([Initiate Job \(POST jobs\)](api-initiate-job-post.md)\)\. The status of the completed job can be `Succeeded` or `Failed`\. The notification sent to the SNS topic is the same output as returned from [Describe Job \(GET JobID\)](api-describe-job-get.md)\.

Amazon SNS topics must grant permission to the vault to be allowed to publish notifications to the topic\.

## Requests<a name="api-vault-notifications-put-requests"></a>

To set notification configuration on your vault, send a PUT request to the URI of the vault's `notification-configuration` subresource\. You specify the configuration in the request body\. The configuration includes the Amazon SNS topic name and an array of events that trigger notification to each topic\.

### Syntax<a name="api-vault-notifications-put-requests-syntax"></a>

```
 1. PUT /AccountId/vaults/VaultName/notification-configuration HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. x-amz-glacier-version: 2012-06-01
 6. 
 7. {
 8.    "SNSTopic": String,
 9.    "Events":[String, ...] 
10. }
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-vault-notifications-put-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-vault-notifications-put-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-vault-notifications-put-requests-elements"></a>

 The JSON in the request body contains the following fields\. 

**Events**  
An array of one or more events for which you want S3 Glacier to send notification\.  
*Valid Values*: `ArchiveRetrievalCompleted` \| `InventoryRetrievalCompleted`   
*Required*: yes  
*Type*: Array

**SNSTopic**  
The Amazon SNS topic ARN\. For more information, go to [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/gsg/Welcome.html) in the *Amazon Simple Notification Service Getting Started Guide*\.  
*Required*: yes  
*Type*: String

## Responses<a name="api-vault-notifications-put-responses"></a>

In response, Amazon S3 Glacier \(S3 Glacier\) returns `204 No Content` if the notification configuration is accepted\.

### Syntax<a name="api-vault-notifications-put-responses-elements"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-vault-notifications-put-responses-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Response Body<a name="api-vault-notifications-put-responses-body"></a>

This operation does not return a response body\.

### Errors<a name="api-vault-notifications-put-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-vault-notifications-put-examples"></a>

The following example demonstrates how to configure vault notification\.

### Example Request<a name="api-vault-notifications-put-example-request"></a>

The following request sets the `examplevault` notification configuration so that notifications for two events \(`ArchiveRetrievalCompleted` and `InventoryRetrievalCompleted` \) are sent to the Amazon SNS topic `arn:aws:sns:us-west-2:012345678901:mytopic`\.

```
 1. PUT /-/vaults/examplevault/notification-policy HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-glacier-version: 2012-06-01
 5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 6. 
 7. { 
 8.    "Events": ["ArchiveRetrievalCompleted", "InventoryRetrievalCompleted"],
 9.    "SNSTopic": "arn:aws:sns:us-west-2:012345678901:mytopic"       
10. }
```

### Example Response<a name="api-vault-notifications-put-example-response"></a>

A successful response returns a `204 No Content`\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-vault-notifications-put"></a>
+ [Get Vault Notifications \(GET notification\-configuration\)](api-vault-notifications-get.md)
+ [Delete Vault Notifications \(DELETE notification\-configuration\)](api-vault-notifications-delete.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)

## See Also<a name="api-vault-notifications-put_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/set-vault-notifications.html) 