# Delete Archive \(DELETE archive\)<a name="api-archive-delete"></a>

## Description<a name="api-archive-delete-description"></a>

This operation deletes an archive from a vault\. You can delete one archive at a time from a vault\. To delete the archive you must provide its archive ID in the delete request\. You can get the archive ID by downloading the vault inventory for the vault that contains the archive\. For more information about downloading the vault inventory, see [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)\.

After you delete an archive, you might still be able to make a successful request to initiate a job to retrieve the deleted archive, but the archive retrieval job will fail\. 

Archive retrievals that are in progress for an archive ID when you delete the archive might or might not succeed according to the following scenarios:
+ If the archive retrieval job is actively preparing the data for download when Amazon S3 Glacier \(S3 Glacier\) receives the delete archive request, the archival retrieval operation might fail\. 
+ If the archive retrieval job has successfully prepared the archive for download when S3 Glacier receives the delete archive request, you will be able to download the output\. 

For more information about archive retrieval, see [Downloading an Archive in Amazon S3 Glacier](downloading-an-archive.md)\. 

This operation is idempotent\. Attempting to delete an already\-deleted archive does not result in an error\. 

## Requests<a name="api-archive-delete-requests"></a>

To delete an archive you send a `DELETE` request to the archive resource URI\.

### Syntax<a name="api-archive-delete-requests-syntax"></a>

```
1. DELETE /AccountId/vaults/VaultName/archives/ArchiveID HTTP/1.1
2. Host: glacier.Region.amazonaws.com
3. x-amz-Date: Date
4. Authorization: SignatureValue
5. x-amz-glacier-version: 2012-06-01
```

**Note**  
The `AccountId` value is the AWS account ID of the account that owns the vault\. You can either specify an AWS account ID or optionally a single '`-`' \(hyphen\), in which case Amazon S3 Glacier uses the AWS account ID associated with the credentials used to sign the request\. If you use an account ID, do not include any hyphens \('\-'\) in the ID\.

### Request Parameters<a name="api-archive-delete-requests-parameters"></a>

This operation does not use request parameters\.

### Request Headers<a name="api-archive-delete-requests-headers"></a>

This operation uses only request headers that are common to all operations\. For information about common request headers, see [Common Request Headers](api-common-request-headers.md)\.

### Request Body<a name="api-archive-delete-requests-elements"></a>

This operation does not have a request body\.

## Responses<a name="api-archive-delete-responses"></a>

### Syntax<a name="api-archive-delete-responses-syntax"></a>

```
HTTP/1.1 204 No Content
x-amzn-RequestId: x-amzn-RequestId
Date: Date
```

### Response Headers<a name="api-archive-delete-responses-headers"></a>

This operation uses only response headers that are common to most responses\. For information about common response headers, see [Common Response Headers](api-common-response-headers.md)\.

### Response Body<a name="api-archive-delete-responses-elements"></a>

This operation does not return a response body\.

### Errors<a name="api-archive-delete-responses-errors"></a>

For information about Amazon S3 Glacier exceptions and error messages, see [Error Responses](api-error-responses.md)\.

## Examples<a name="api-archive-delete-examples"></a>

The following example demonstrates how to delete an archive from the vault named `examplevault`\.

### Example Request<a name="api-archive-delete-example-request"></a>

The ID of the archive to be deleted is specified as a subresource of `archives`\.

```
1. DELETE /-/vaults/examplevault/archives/NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId HTTP/1.1
2. Host: glacier.us-west-2.amazonaws.com
3. x-amz-Date: 20170210T120000Z
4. x-amz-glacier-version: 2012-06-01
5. Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20141123/us-west-2/glacier/aws4_request,SignedHeaders=host;x-amz-date;x-amz-glacier-version,Signature=9257c16da6b25a715ce900a5b45b03da0447acf430195dcb540091b12966f2a2
```

### Example Response<a name="api-archive-delete-example-response"></a>

If the request is successful, S3 Glacier responds with `204 No Content` to indicate that the archive is deleted\.

```
1. HTTP/1.1 204 No Content
2. x-amzn-RequestId: AAABZpJrTyioDC_HsOmHae8EZp_uBSJr6cnGOLKp_XJCl-Q
3. Date: Wed, 10 Feb 2017 12:00:00 GMT
```

## Related Sections<a name="related-sections-archive-delete"></a>
+ [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md)
+ [Upload Archive \(POST archive\)](api-archive-post.md)
+ [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)