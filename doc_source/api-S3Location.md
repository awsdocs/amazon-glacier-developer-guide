# S3Location<a name="api-S3Location"></a>

 Contains information about the location in Amazon S3 where the job results are stored\.

## Contents<a name="api-S3Location-contents"></a>

**AccessControlList**  
A list of grants that control access to the stored results\.  
*Type*: Array of [Grant](api-Grant.md) objects  
*Required*: no

**BucketName**  
The name of the Amazon S3 bucket where the job results are stored\. The bucket must be in the same AWS Region as the vault that contains the input archive object\.  
*Type*: String  
*Required*: yes

**CannedACL**  
The canned access control list \(ACL\) to apply to the job results\.  
*Type*: String  
*Valid Values*: `private` \| `public-read` \| `public-read-write` \| `aws-exec-read` \| `authenticated-read` \| `bucket-owner-read` \| `bucket-owner-full-control`  
*Required*: no

**Encryption**  
An object that contains information about the encryption used to store the job results in Amazon S3\.  
*Type*: [Encryption](api-Encryption.md) object  
*Required*: no

**Prefix**  
The prefix that is prepended to the results for this request\. The maximum length for the prefix is 512 bytes\.  
*Type*: String  
*Required*: yes

**StorageClass**  
The class of storage used to store the job results\.  
*Type*: String  
*Valid Values*: `STANDARD` \| `REDUCED_REDUNDANCY` \| `STANDARD_IA`  
*Required*: no

**Tagging**  
The tag set that is applied to the job results\.  
*Type*: String to string map  
*Required*: no

**UserMetadata**  
A map  of metadata to store with the job results in Amazon S3\.  
*Type*: String to string map  
*Required*: no

## More Info<a name="more-info-api-S3Location"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)