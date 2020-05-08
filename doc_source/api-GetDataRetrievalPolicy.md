# Get Data Retrieval Policy \(GET policy\)<a name="api-GetDataRetrievalPolicy"></a>

## Description<a name="api-GetDataRetrievalPolicy-description"></a>

This operation returns the current data retrieval policy for the AWS account and AWS Region specified in the `GET` request\. For more information about data retrieval policies, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\.

## Requests<a name="api-GetDataRetrievalPolicy-requests"></a>

To return the current data retrieval policy, send an HTTP `GET` request to the data retrieval policy URI as shown in the following syntax example\.

### Syntax<a name="api-GetDataRetrievalPolicy-requests-syntax"></a>

```
1. GET /AccountId/policies/data-retrieval HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-GetDataRetrievalPolicy-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-GetDataRetrievalPolicy-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-GetDataRetrievalPolicy-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-GetDataRetrievalPolicy-responses"></a>

### Syntax<a name="api-GetDataRetrievalPolicy-responses-syntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: length
{
  "Policy":
    {
      "Rules":[
         {
            "BytesPerHour": Number,
            "Strategy": String	 
         }
       ]
    }
}
```

### Response Headers<a name="api-GetDataRetrievalPolicy-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-GetDataRetrievalPolicy-responses-elements"></a>

The response body contains the following JSON fields\.

**BytesPerHour**  
The maximum number of bytes that can be retrieved in an hour\.  
This field will be present only if the value of the **Strategy** field is `BytesPerHour`\.   
*Type*: Number

**Rules**  
The policy rule\. Although this is a list type, currently there will be only one rule, which contains a Strategy field and optionally a BytesPerHour field\.  
*Type*: Array

**Strategy**  
The type of data retrieval policy\.  
*Type*: String  
Valid values: `BytesPerHour`\|`FreeTier`\|`None`\. `BytesPerHour` is equivalent to selecting **Max Retrieval Rate** in the console\. `FreeTier` is equivalent to selecting **Free Tier Only** in the console\. `None` is equivalent to selecting **No Retrieval Policy** in the console\. For more information about selecting data retrieval policies in the console, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\.

### Errors<a name="api-GetDataRetrievalPolicy-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-GetDataRetrievalPolicy-examples"></a>

The following example demonstrates how to get a data retrieval policy\.

### Example Request<a name="api-GetDataRetrievalPolicy-example-request"></a>

In this example, a `GET` request is sent to the URI of a policy's location\.

```
1. GET /-/policies/data-retrieval HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-GetDataRetrievalPolicy-example-response"></a>

A successful response shows the data retrieval policy in the body of the response in JSON format\. 

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:00:00 GMT
 4. Content-Type: application/json
 5. Content-Length: 85
 6.  
 7. {
 8.   "Policy":
 9.     {
10.       "Rules":[
11.          {
12.            "BytesPerHour":10737418240,
13.            "Strategy":"BytesPerHour"
14.           }
15.        ]
16.     }
17. }
```

## Related Sections<a name="related-sections-GetDataRetrievalPolicy"></a>
+ [Set Data Retrieval Policy \(PUT policy\)](api-SetDataRetrievalPolicy.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)