# Purchase Provisioned Capacity \(POST provisioned\-capacity\)<a name="api-PurchaseProvisionedCapacity"></a>

This operation purchases a provisioned capacity unit for an AWS account\. 

A provisioned capacity unit lasts for one month starting at the date and time of purchase, which is the start date\. The unit expires on the expiration date, which is exactly one month after the start date to the nearest second\. 

If the start date is on the 31st day of a month, the expiration date is the last day of the next month\. For example, if the start date is August 31, the expiration date is September 30\. If the start date is January 31, the expiration date is February 28\.

Provisioned capacity helps ensure that your retrieval capacity for expedited retrievals is available when you need it\. Each unit of capacity ensures that at least three expedited retrievals can be performed every five minutes and provides up to 150 MB/s of retrieval throughput\. For more information about provisioned capacity, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)\. 

## Requests<a name="api-PurchaseProvisionedCapacity-requests"></a>

To purchase provisioned capacity unit for an AWS account send an HTTP `POST` request to the provisioned\-capacity URI\.

### Syntax<a name="api-PurchaseProvisionedCapacity-requests-syntax"></a>

```
1. POST /AccountId/provisioned-capacity HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. Content-Length: Length
6. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-PurchaseProvisionedCapacity-requestParameters"></a>

#### Request Headers<a name="api-PurchaseProvisionedCapacity-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

#### Request Body<a name="api-PurchaseProvisionedCapacity-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-PurchaseProvisionedCapacity-responses"></a>

If the operation request is successful, the service returns an HTTP `201 Created` response\.

### Syntax<a name="api-PurchaseProvisionedCapacity-response-syntax"></a>

```
HTTP/1.1 201 Created
x-amzn-RequestId: x-amzn-RequestId
Date: Date
x-amz-capacity-id: CapacityId
```

### Response Headers<a name="api-PurchaseProvisionedCapacity-responses-headers"></a>

A successful response includes the following response headers, in addition to the response headers that are common to all operations\. For more information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.


|  Name  |  Description | 
| --- | --- | 
|  `x-amz-capacity-id`   |  The ID that identifies the provisioned capacity unit\. Type: String  | 

### Response Body<a name="api-PurchaseProvisionedCapacity-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-PurchaseProvisionedCapacity-responses-errors"></a>

This operation includes the following error or errors, in addition to the possible errors common to all Amazon S3 Glacier operations\. For information about Amazon S3 Glacier errors and a list of error codes, see [Error Responses](api-error-responses.md)\.


| Code | Description | HTTP Status Code | Type | 
| --- | --- | --- | --- | 
| LimitExceededException | Returned if the given request would exceed the account's limit of provisioned capacity units\.  | 400 Bad Request | Client | 

## Examples<a name="api-PurchaseProvisionedCapacity-examples"></a>

The following example purchases provisioned capacity for an account\.

### Example Request<a name="api-PurchaseProvisionedCapacity-example-request"></a>

The following example sends an HTTP POST request to purchase a provisioned capacity unit\. 

```
1. POST /123456789012/provisioned-capacity HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
5. Content-Length: length
6. x-amz-glacier-version: 2012-06-01
```

### Example Response<a name="api-PurchaseProvisionedCapacity-example-response"></a>

If the request was successful, Amazon S3 Glacier \(S3 Glacier\) returns an `HTTP 201 Created` response, as shown in the following example\.

```
1. HTTP/1.1 201 Created
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
4. x-amz-capacity-id: zSaq7NzHFQDANTfQkDen4V7z
```

## Related Sections<a name="api-PurchaseProvisionedCapacity-related-sections"></a>
+ [List Provisioned Capacity \(GET provisioned\-capacity\)](api-ListProvisionedCapacity.md)