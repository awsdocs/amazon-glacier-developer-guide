# List Provisioned Capacity \(GET provisioned\-capacity\)<a name="api-ListProvisionedCapacity"></a>

This operation lists the provisioned capacity units for the specified AWS account\. For more information about provisioned capacity, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)\. 

A provisioned capacity unit lasts for one month starting at the date and time of purchase, which is the start date\. The unit expires on the expiration date, which is exactly one month after the start date to the nearest second\. 

If the start date is on the 31st day of a month, the expiration date is the last day of the next month\. For example, if the start date is August 31, the expiration date is September 30\. If the start date is January 31, the expiration date is February 28\. You can see this functionality in the [Example Response](#api-ListProvisionedCapacity-example1-response)\.

## Request Syntax<a name="api-ListProvisionedCapacity-RequestSyntax"></a>

To list the provisioned retrieval capacity for an account, send an HTTP GET request to the provisioned\-capacity URI as shown in the following syntax example\.

```
1. GET /AccountId/provisioned-capacity HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

## Request Parameters<a name="api-ListProvisionedCapacity-RequestParameters"></a>

This operation does not use request parameters\.

## Request Headers<a name="api-ListProvisionedCapacity-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

## Request Body<a name="api-ListProvisionedCapacity-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-ListProvisionedCapacity-responses"></a>

If the operation is successful, the service sends back an HTTP `200 OK` response\.

### Response Syntax<a name="api-ListProvisionedCapacity-ResponseSyntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: Length
{
   "ProvisionedCapacityList": 
      {
         "CapacityId" : "string",
         "StartDate" : "string"
         "ExpirationDate" : "string"
      }
}
```

### Response Headers<a name="api-ListProvisionedCapacity-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-ListProvisionedCapacity-body"></a>

The response body contains the following JSON fields\.

**CapacityId**  <a name="Glacier-ListProvisionedCapacity-response"></a>
The ID that identifies the provisioned capacity unit\.  
 *Type*: String\.

**StartDate**  
The date that the provisioned capacity unit was purchased, in Coordinated Universal Time \(UTC\)\.  
*Type*: String\. A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

**ExpirationDate**  
The date that the provisioned capacity unit expires, in Coordinated Universal Time \(UTC\)\.  
*Type*: String\. A string representation in the ISO 8601 date format, for example `2013-03-20T17:03:43.221Z`\.

### Errors<a name="api-ListProvisionedCapacity-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-ListProvisionedCapacity-examples"></a>

The following example lists the provisioned capacity units for an account\.

### Example Request<a name="api-ListProvisionedCapacity-example1-request"></a>

In this example, a GET request is sent to retrieve a list of the provisioned capacity units for the specified account\.

```
1. GET /123456789012/priority-capacity HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-ListProvisionedCapacity-example1-response"></a>

If the request was successful, Amazon S3 Glacier \(S3 Glacier\) returns a `HTTP 200 OK` with a list of provisioned capacity units for the account as shown in the following example\.

 The provisioned capacity unit listed first is an example of a unit with a start date of January 31, 2017 and expiration date of February 28, 2017\. As stated earlier, if the start date is on the 31st day of a month, the expiration date is the last day of the next month\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:02:00 GMT
 4. Content-Type: application/json
 5. Content-Length: length
 6. 
 7. {
 8.    "ProvisionedCapacityList",
 9.       {
10.          "CapacityId": "zSaq7NzHFQDANTfQkDen4V7z",
11.          "StartDate": "2017-01-31T14:26:33.031Z",
12.          "ExpirationDate": "2017-02-28T14:26:33.000Z",
13.       },
14.       {
15.          "CapacityId": "yXaq7NzHFQNADTfQkDen4V7z",
16.          "StartDate": "2016-12-13T20:11:51.095Z"",
17.          "ExpirationDate": "2017-01-13T20:11:51.000Z" ",
18.       },
19.       ...
20. }
```

## Related Sections<a name="api-ListProvisionedCapacity-related-sections"></a>
+ [Purchase Provisioned Capacity \(POST provisioned\-capacity\)](api-PurchaseProvisionedCapacity.md)