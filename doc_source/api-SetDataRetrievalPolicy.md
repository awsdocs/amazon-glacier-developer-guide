# Set Data Retrieval Policy \(PUT policy\)<a name="api-SetDataRetrievalPolicy"></a>

## Description<a name="api-SetDataRetrievalPolicy-description"></a>

This operation sets and then enacts a data retrieval policy in the AWS Region specified in the `PUT` request\. You can set one policy per AWS Region for an AWS account\. The policy is enacted within a few minutes of a successful `PUT` operation\. 

 The set policy operation does not affect retrieval jobs that were in progress before the policy was enacted\. For more information about data retrieval policies, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\. 

## Requests<a name="api-SetDataRetrievalPolicy-requests"></a>

### Syntax<a name="api-SetDataRetrievalPolicy-requests-syntax"></a>

To set a data retrieval policy, send an HTTP PUT request to the data retrieval policy URI as shown in the following syntax example\.

```
 1. PUT /AccountId/policies/data-retrieval HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. Content-Length: Length
 6. x-amz-glacier-version: 2012-06-01
 7. 			
 8. {
 9.   "Policy":
10.     {
11.       "Rules":[
12.          {
13.              "Strategy": String,
14.              "BytesPerHour": Number          
15.          }
16.        ]
17.     }
18. }
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-SetDataRetrievalPolicy-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-SetDataRetrievalPolicy-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-SetDataRetrievalPolicy-requests-elements"></a>

The request body contains the following JSON fields\.

**BytesPerHour**  
The maximum number of bytes that can be retrieved in an hour\.  
This field is required only if the value of the Strategy field is `BytesPerHour`\. Your PUT operation will be rejected if the Strategy field is not set to `BytesPerHour` and you set this field\.  
*Type*: Number  
*Required*: Yes, if the Strategy field is set to `BytesPerHour`\. Otherwise, no\.  
*Valid Values*: Minimum integer value of 1\. Maximum integer value of 2^63 \- 1 inclusive\.

**Rules**  
The policy rule\. Although this is a list type, currently there must be only one rule, which contains a Strategy field and optionally a BytesPerHour field\.  
*Type*: Array  
*Required*: Yes

**Strategy**  
The type of data retrieval policy to set\.  
*Type*: String  
*Required*: Yes  
Valid values: `BytesPerHour`\|`FreeTier`\|`None`\. `BytesPerHour` is equivalent to selecting **Max Retrieval Rate** in the console\. `FreeTier` is equivalent to selecting **Free Tier Only** in the console\. `None` is equivalent to selecting **No Retrieval Policy** in the console\. For more information about selecting data retrieval policies in the console, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\.

## Responses<a name="api-SetDataRetrievalPolicy-responses"></a>

### Syntax<a name="api-SetDataRetrievalPolicyresponse-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-SetDataRetrievalPolicy-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-SetDataRetrievalPolicy-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-SetDataRetrievalPolicy-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-SetDataRetrievalPolicy-examples"></a>

### Example Request<a name="api-SetDataRetrievalPolicy-example-request"></a>

The following example sends an HTTP PUT request with the Strategy field set to `BytesPerHour`\. 

```
 1. PUT /-/policies/data-retrieval HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-glacier-version: 2012-06-01
 5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 6. 			
 7. {
 8.   "Policy":
 9.     {
10.       "Rules":[
11.          {
12.              "Strategy":"BytesPerHour",
13.              "BytesPerHour":10737418240       
14.           }
15.        ]
16.     }
17. }
```

The following example sends an HTTP PUT request with the Strategy field set to `FreeTier`\. 

```
 1. PUT /-/policies/data-retrieval HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-glacier-version: 2012-06-01
 5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 6. 			
 7. {
 8.   "Policy":
 9.     {
10.       "Rules":[
11.          {
12.              "Strategy":"FreeTier"   
13.           }
14.        ]
15.     }
16. }
```

The following example sends an HTTP PUT request with the Strategy field set to `None`\. 

```
 1. PUT /-/policies/data-retrieval HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. x-amz-glacier-version: 2012-06-01
 5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 6. 			
 7. {
 8.   "Policy":
 9.     {
10.       "Rules":[
11.          {
12.              "Strategy":"None"   
13.           }
14.        ]
15.     }
16. }
```

### Example Response<a name="api-SetDataRetrievalPolicy-example-response"></a>

If the request was successful Amazon S3 Glacier \(S3 Glacier\) sets the policy and returns a `HTTP 204 No Content` as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
```

## Related Sections<a name="related-sections-SetDataRetrievalPolicy"></a>
+ [Get Data Retrieval Policy \(GET policy\)](api-GetDataRetrievalPolicy.md)
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)