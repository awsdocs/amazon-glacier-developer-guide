# Error Responses<a name="api-error-responses"></a>

In the event of an error, the API returns one of the following exceptions:


| Code | Description | HTTP Status Code | Type | 
| --- | --- | --- | --- | 
| AccessDeniedException | Returned if there was an attempt to access a resource not allowed by an AWS Identity and Access Management \(IAM\) policy, or the incorrect AWS Account ID was used in the request URI\. For more information, see [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)\. | 403 Forbidden | Client | 
| BadRequest | Returned if the request cannot be processed\.  | 400 Bad Request | Client | 
| ExpiredTokenException | Returned if the security token used in the request has expired\.  | 403 Forbidden | Client | 
| InsufficientCapacityException | Returned if there is insufficient capacity to process the expedited request\. This error only applies to expedited retrievals and not to standard or bulk retrievals\. | 503 Service Unavailable | Server | 
| InvalidParameterValueException | Returned if a parameter of the request is incorrectly specified\. | 400 Bad Request | Client | 
| InvalidSignatureException | Returned if the request signature is invalid\. | 403 Forbidden | Client | 
| LimitExceededException | Returned if the request results in one of the following limits being exceeded, a vault limit, a tags limit, or the provisioned capacity limit\. | 400 Bad Request | Client | 
| MissingAuthenticationTokenException | Returned if no authentication data is found for the request\. | 400 Bad Request | Client | 
| MissingParameterValueException | Returned if a required header or parameter is missing from the request\. | 400 Bad Request | Client | 
| PolicyEnforcedException | Returned if a retrieval job will exceed the current data policy's retrieval rate limit\. For more information about data retrieval policies, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\. | 400 Bad Request | Client | 
| ResourceNotFoundException | Returned if the specified resource such as a vault, upload ID, or job ID does not exist\. | 404 Not Found | Client | 
| RequestTimeoutException | Returned if uploading an archive and Amazon S3 Glacier \(S3 Glacier\) times out while receiving the upload\. | 408 Request Timeout | Client | 
| SerializationException | Returned if the body of the request is invalid\. If including a JSON payload, check that it is well\-formed\. | 400 Bad Request | Client | 
| ServiceUnavailableException | Returned if the service cannot complete the request\. | 500 Internal Server Error | Server | 
| ThrottlingException | Returned if you need to reduce your rate of requests to S3 Glacier\. | 400 Bad Request | Client | 
| UnrecognizedClientException | Returned if the Access Key ID or security token is invalid\. | 400 Bad Request | Client | 

Various S3 Glacier APIs return the same exception, but with different exception messages to help you troubleshoot the specific error encountered\.

S3 Glacier returns error information in the response body\. The following examples show some of the error responses\.

## Example 1: Describe Job request with a job ID that does not exist<a name="bad-request-error-example1"></a>

Suppose you send a [Describe Job \(GET JobID\)](api-describe-job-get.md) request for a job that does not exist\. That is, you specify a job ID that does not exist\. 

```
1. GET /-/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVEXAMPLEbadJobID HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

In response, S3 Glacier returns the following error response\. 

```
HTTP/1.1 404 Not Found
x-amzn-RequestId: AAABaZ9N92Iiyv4N7sru3ABEpSQkuFtmH3NP6aAC51ixfjg
Content-Type: application/json
Content-Length: 185
Date: Wed, 10 Feb 2017 12:00:00 GMT
{
  "code": "ResourceNotFoundException",
  "message": "The job ID was not found: HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVEXAMPLEbadJobID",
  "type": "Client"
  }
```

Where:

**Code**  
One of the general exceptions\.  
*Type*: String

**Message**  
A generic description of the error condition specific to the API that returns the error\.  
*Type*: String

**Type**  
The source of the error\. The field can be one of the following values: `Client`, `Server`, or `Unknown`\.  
*Type*: String\.

Note the following in the preceding response:
+ For the error response, S3 Glacier returns status code values of `4xx` and `5xx`\. In this example, the status code is `404 Not Found`\. 
+ The `Content-Type` header value `application/json` indicates JSON in the body
+ The JSON in the body provides the error information\.

In the previous request, instead of a bad job ID, suppose you specify a vault that does not exist\. The response returns a different message\.

```
HTTP/1.1 404 Not Found
x-amzn-RequestId: AAABBeC9Zw0rp_5D0L8VfB3FA_WlTupqTKAUehMcPhdgni0
Content-Type: application/json
Content-Length: 154
Date: Wed, 10 Feb 2017 12:00:00 GMT
{
  "code": "ResourceNotFoundException",
  "message": "Vault not found for ARN: arn:aws:glacier:us-west-2:012345678901:vaults/examplevault",
  "type": "Client"
}
```

## Example 2: List Jobs request with an invalid value for the request parameter<a name="bad-request-error-example2"></a>

In this example you send a [List Jobs \(GET jobs\)](api-jobs-get.md) request to retrieve vault jobs with a specific `statuscode`, and you provide an incorrect `statuscode` value `finished`, instead of the acceptable values `InProgress`, `Succeeded`, or `Failed`\. 

```
GET /-/vaults/examplevault/jobs?statuscode=finished HTTP/1.1 
Host: glacier.us-west-2.amazonaws.com 
Date: 20170210T120000Z
x-amz-glacier-version: 2012-06-01
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

S3 Glacier returns the `InvalidParameterValueException` with an appropriate message\.

```
HTTP/1.1 400 Bad Request
x-amzn-RequestId: AAABaZ9N92Iiyv4N7sru3ABEpSQkuFtmH3NP6aAC51ixfjg
Content-Type: application/json
Content-Length: 141
Date: Wed, 10 Feb 2017 12:00:00 GMT
{
  "code": "InvalidParameterValueException",
  "message": "The job status code is not valid: finished",
  "type: "Client"
}
```