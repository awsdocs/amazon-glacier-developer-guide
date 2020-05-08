# API Reference for Amazon S3 Glacier<a name="amazon-glacier-api"></a>

Amazon S3 Glacier \(S3 Glacier\) supports a set of operations—specifically, a set of RESTful API calls—that enable you to interact with the service\. 

You can use any programming library that can send HTTP requests to send your REST requests to S3 Glacier\. When sending a REST request, S3 Glacier requires that you authenticate every request by signing the request\. Additionally, when uploading an archive, you must also compute the checksum of the payload and include it in your request\. For more information, see [Signing Requests](amazon-glacier-signing-requests.md)\.

If an error occurs, you need to know what S3 Glacier sends in an error response so that you can process it\. This section provides all this information, in addition to documenting the REST operations, so that you can make REST API calls directly\. 

You can either use the REST API calls directly or use the AWS SDKs that provide wrapper libraries to simplify your coding task\. These libraries sign each request you send and compute the checksum of the payload in your request\. Therefore, using the AWS SDKs simplifies your coding task\. This developer guide provides working examples of basic S3 Glacier operations using the AWS SDK for Java and \.NET\. For more information see, [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)\.

**Topics**
+ [Common Request Headers](api-common-request-headers.md)
+ [Common Response Headers](api-common-response-headers.md)
+ [Signing Requests](amazon-glacier-signing-requests.md)
+ [Computing Checksums](checksum-calculations.md)
+ [Error Responses](api-error-responses.md)
+ [Vault Operations](vault-operations.md)
+ [Archive Operations](archive-operations.md)
+ [Multipart Upload Operations](multipart-archive-operations.md)
+ [Job Operations](job-operations.md)
+ [Data Types Used in Job Operations](api-data-types.md)
+ [Data Retrieval Operations](data-retrieval-policy-operations.md)