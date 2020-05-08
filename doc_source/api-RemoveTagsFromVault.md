# Remove Tags From Vault \(POST tags remove\)<a name="api-RemoveTagsFromVault"></a>

This operation removes one or more tags from the set of tags attached to a vault\. For more information about tags, see [Tagging Amazon S3 Glacier Resources](tagging.md)\.

This operation is idempotent\. The operation will be successful, even if there are no tags attached to the vault\. 

## Request Syntax<a name="api-RemoveTagsFromVault-RequestSyntax"></a>

To remove tags from a vault, send an HTTP POST request to the tags URI as shown in the following syntax example\.

```
POST /AccountId/vaults/vaultName/tags?operation=remove HTTP/1.1
Host: glacier.Region.amazonaws.com
Date: Date
Authorization: SignatureValue
Content-Length: Length
x-amz-glacier-version: 2012-06-01
{
   "TagKeys": [
      "string",
      "string"
   ]
}
```

**Note**  
The `AccountId` value is the AWS account ID\. This value must match the AWS account ID associated with the credentials used to sign the request\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you specify your account ID, do not include any hyphens \('\-'\) in the ID\.

## Request Parameters<a name="api-RemoveTagsFromVault-RequestParameters"></a>


|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
|  operation=remove  |  A single query string parameter `operation` with a value of `remove` to distinguish it from [Add Tags To Vault \(POST tags add\)](api-AddTagsToVault.md)\.  |  Yes  | 

### Request Headers<a name="api-RemoveTagsFromVault-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-RemoveTagsFromVault-requests-elements"></a>

The request body contains the following JSON fields\.

**TagKeys**  
A list of tag keys\. Each corresponding tag is removed from the vault\.  
 *Type:* array of Strings  
 *Length constraint:* Minimum of 1 item in the list\. Maximum of 10 items in the list\.  
 *Required:* Yes

## Responses<a name="api-RemoveTagsFromVault-responses"></a>

If the action is successful, the service sends back an HTTP `204 No Content` response with an empty HTTP body\.

### Syntax<a name="api-RemoveTagsFromVault-response-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-RemoveTagsFromVault-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-RemoveTagsFromVault-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-RemoveTagsFromVault-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-RemoveTagsFromVault-examples"></a>

### Example Request<a name="api-RemoveTagsFromVault-example-request"></a>

The following example sends an HTTP POST request to remove the specified tags\.

```
 1. POST /-/vaults/examplevault/tags?operation=remove HTTP/1.1
 2. Host: glacier.us-west-2.amazonaws.com
 3. x-amz-Date: 20170210T120000Z
 4. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
 5. Content-Length: length
 6. x-amz-glacier-version: 2012-06-01
 7. 			
 8. {
 9.    "TagsKeys": [
10.       "examplekey1",
11.       "examplekey2"
12.    ]        
13. }
```

### Example Response<a name="api-RemoveTagsFromVault-example-response"></a>

If the request was successful Amazon S3 Glacier \(S3 Glacier\) returns a `HTTP 204 No Content` as shown in the following example\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:02:00 GMT
```

## Related Sections<a name="related-sections-RemoveTagsFromVault"></a>
+ [Add Tags To Vault \(POST tags add\)](api-AddTagsToVault.md)
+ [List Tags For Vault \(GET tags\)](api-ListTagsForVault.md)

## See Also<a name="api-RemoveTagsFromVault_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/reference/glacier/remove-tags-from-vault.html) 