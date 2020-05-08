# List Tags For Vault \(GET tags\)<a name="api-ListTagsForVault"></a>

This operation lists all the tags attached to a vault\. The operation returns an empty map if there are no tags\. For more information about tags, see [Tagging Amazon S3 Glacier Resources](tagging.md)\.

## Request Syntax<a name="api-ListTagsForVault-RequestSyntax"></a>

To list the tags for a vault, send an HTTP GET request to the tags URI as shown in the following syntax example\.

```
1. GET /AccountId/vaults/vaultName/tags HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

## Request Parameters<a name="api-ListTagsForVault-RequestParameters"></a>

This operation does not use request parameters\.

## Request Headers<a name="api-ListTagsForVault-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

## Request Body<a name="api-ListTagsForVault-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-ListTagsForVault-responses"></a>

If the operation is successful, the service sends back an HTTP `200 OK` response\.

### Response Syntax<a name="api-ListTagsForVault-ResponseSyntax"></a>

```
HTTP/1.1 200 OK
x-amzn-RequestId: x-amzn-RequestId
Date: Date
Content-Type: application/json
Content-Length: Length
{
   "Tags": 
      {
         "string" : "string",
         "string" : "string"
      }
}
```

### Response Headers<a name="api-ListTagsForVault-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-ListTagsForVault-body"></a>

The response body contains the following JSON fields\.

**Tags**  <a name="Glacier-ListTagsForVault-response-Tags"></a>
The tags attached to the vault\. Each tag is composed of a key and a value\.  
 *Type:* String to String map   
 *Required:* Yes 

### Errors<a name="api-ListTagsForVault--errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-ListTagsForVault-examples"></a>

### Example: List Tags For a Vault<a name="api-ListTagsForVault-example1"></a>

The following example lists the tags for a vault\.

#### Example Request<a name="api-ListTagsForVault-example1-request"></a>

In this example, a GET request is sent to retrieve a list of tags from the specified vault\.

```
1. GET /-/vaults/examplevault/tags HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

#### Example Response<a name="api-ListTagsForVault-example1-response"></a>

If the request was successful, Amazon S3 Glacier \(S3 Glacier\) returns a `HTTP 200 OK` with a list of tags for the vault as shown in the following example\.

```
 1. HTTP/1.1 200 OK
 2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
 3. Date: Wed, 10 Feb 2017 12:02:00 GMT
 4. Content-Type: application/json
 5. Content-Length: length
 6. 
 7. {
 8.    "Tags",
 9.       {
10.          "examplekey1": "examplevalue1",
11.          "examplekey2": "examplevalue2"
12.       }  
13. }
```

## Related Sections<a name="related-sections-ListTagsForVault"></a>
+ [Add Tags To Vault \(POST tags add\)](api-AddTagsToVault.md)
+ [Remove Tags From Vault \(POST tags remove\)](api-RemoveTagsFromVault.md)

## See Also<a name="api-ListTagsForVault_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/list-tags-for-vault.html) 