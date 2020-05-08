# Add Tags To Vault \(POST tags add\)<a name="api-AddTagsToVault"></a>

This operation adds the specified tags to a vault\. Each tag is composed of a key and a value\. Each vault can have up to 50 tags\. If your request would cause the tag limit for the vault to be exceeded, the operation throws the `LimitExceededException` error\.

If a tag already exists on the vault under a specified key, the existing key value will be overwritten\. For more information about tags, see [Tagging Amazon S3 Glacier Resources](tagging.md)\.

## Request Syntax<a name="api-AddTagsToVault-requestSyntax"></a>

To add tags to a vault, send an HTTP POST request to the tags URI as shown in the following syntax example\.

```
 1. POST /AccountId/vaults/vaultName/tags?operation=add HTTP/1.1
 2. Host: glacier.Region.amazonaws.com
 3. Date: Date
 4. Authorization: SignatureValue
 5. Content-Length: Length
 6. x-amz-glacier-version: 2012-06-01
 7. 			
 8. {
 9.    "Tags": 
10.       {
11.          "string": "string",
12.          "string": "string"
13.       }        
14. }
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

## Request Parameters<a name="api-AddTagsToVault-requestParameters"></a>


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  operation=add  |  A single query string parameter `operation` with a value of `add` to distinguish it from [Remove Tags From Vault \(POST tags remove\)](api-RemoveTagsFromVault.md)\.  |  Yes  | 

### Request Headers<a name="api-AddTagsToVault-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-AddTagsToVault-requests-elements"></a>

The request body contains the following JSON fields\.

**Tags**  
The tags to add to the vault\. Each tag is composed of a key and a value\. The value can be an empty string\.  
 *Type:* String to String map   
 *Length constraints:* Minimum length of 1\. Maximum length 10\.  
 *Required:* Yes 

## Responses<a name="api-AddTagsToVault-responses"></a>

If the operation request is successful, the service returns an HTTP `204 No Content` response\.

### Syntax<a name="api-AddTagsToVault-response-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-AddTagsToVault-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-AddTagsToVault-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-AddTagsToVault-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-AddTagsToVault-examples"></a>

### Example Request<a name="api-AddTagsToVault-example-request"></a>

The following example sends an HTTP POST request with the tags to add to the vault\.

```
 1. POST /-/vaults/examplevault/tags?operation=add HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 5. Content-Length: length
 6. x-amz-glacier-version: 2012-06-01
 7. 			
 8. {
 9.   "Tags": 
10.     {
11.        "examplekey1": "examplevalue1",
12.        "examplekey2": "examplevalue2"
13.     }        
14. }
```

### Example Response<a name="api-AddTagsToVault-example-response"></a>

If the request was successful S3 Glacier returns a `HTTP 204 No Content` as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
```

## Related Sections<a name="related-sections-AddTagsToVault"></a>
+ [List Tags For Vault \(GET tags\)](api-ListTagsForVault.md)
+ [Remove Tags From Vault \(POST tags remove\)](api-RemoveTagsFromVault.md)

## See Also<a name="api-AddTagsToVault-SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/add-tags-to-vault.html) 