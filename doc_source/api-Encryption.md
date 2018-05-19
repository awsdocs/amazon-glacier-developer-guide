# Encryption<a name="api-Encryption"></a>

Contains information about the encryption used to store the job results in Amazon S3\.

## Contents<a name="api-Encryption-contents"></a>

**Encryption**  
The server\-side encryption algorithm used when storing job results in Amazon S3\. The default is no encryption\.  
*Type*: String  
*Valid Values*: `aws:kms` \| `AES256`  
*Required*: no

**KMSContext**  
Optional\. If the encryption type is `aws:kms,` you can use this value to specify the encryption context for the job results\.  
*Type*: String  
*Required*: no

**KMSKeyId**  
The AWS Key Management Service \(AWS KMS\) key ID to use for object encryption\.  
*Type*: String  
*Required*: no

## More Info<a name="more-info-api-Encryption"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)