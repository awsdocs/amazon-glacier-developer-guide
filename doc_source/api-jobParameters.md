# jobParameters<a name="api-jobParameters"></a>

Provides options for defining a job\.

## Contents<a name="api-jobParameters-contents"></a>

**ArchiveId**  
The ID of the archive that you want\. This field is required if the `Type` field is set to `select` or `archive-retrieval`\. An error occurs if you specify this field for an inventory retrieval job request\.   
*Valid Values*: Must be a valid archive ID that you obtained from a previous request to Amazon S3 Glacier \(S3 Glacier\)\.  
*Type*: String  
*Required*: Yes when `Type` is set to `select` or `archive-retrieval`\.

**Description**  
The optional description for the job\.   
*Valid Values*: The description must be less than or equal to 1,024 bytes\. The allowable characters are 7\-bit ASCII without control codes—specifically, ASCII values 32–126 decimal or 0x20–0x7E hexadecimal\.  
*Type*: String  
*Required*: no

**Format**  
\(Optional\) The output format, when initiating a job to retrieve a vault inventory\. If you are initiating an inventory job and don't specify a `Format` field, JSON is the default format\.  
*Valid Values*: `CSV` \| `JSON`   
*Type*: String  
*Required*: no

**InventoryRetrievalParameters**  
Input parameters used for a range inventory retrieval\.  
*Type*: [InventoryRetrievalJobInput](api-InventoryRetrievalJobInput.md) object  
*Required*: no

**OutputLocation**  
 An object that contains information about the location where the select job results are stored\.   
*Type*: [OutputLocation](api-OutputLocation.md) object  
*Required*: Yes, for `select` jobs\.

**RetrievalByteRange**  
The byte range to retrieve for an `archive-retrieval`, in the form "*StartByteValue*\-*EndByteValue*"\. If this field isn't specified, the whole archive is retrieved\. If this field is specified, the byte range must be megabyte \(1024\*1024\) aligned\. Megabyte\-aligned means that *StartByteValue* must be divisible by 1 MB, and *EndByteValue* plus 1 must be divisible by 1 MB or be the end of the archive specified as the archive byte size value minus 1\. If **RetrievalByteRange** is not megabyte\-aligned, this operation returns a `400` response\.   
An error occurs if you specify this field for an `inventory-retrieval` or `select` job request\.   
*Type*: String  
*Required*: no

**SelectParameters**  
An object that contains information about the parameters used for a select\.  
*Type*: [SelectParameters](api-SelectParameters.md) object  
*Required*: no

**SNSTopic**  
The Amazon Resource Name \(ARN\) of the Amazon SNS topic where S3 Glacier sends a notification when the job is completed and output is ready for you to download\. The specified topic publishes the notification to its subscribers\.   
The SNS topic must exist\. If it doesn't, S3 Glacier doesn't create it for you\. Additionally, the SNS topic must have a policy that allows the account that created the job to publish messages to the topic\. For information about SNS topic names, see [CreateTopic](https://docs.aws.amazon.com/sns/latest/api/API_CreateTopic.html) in the *Amazon Simple Notification Service* *API Reference*\.  
*Type*: String  
*Required*: no

**Tier**  
The tier to use for a select or an archive retrieval job\. `Standard` is the default value used\.  
*Valid Values*: `Expedited` \| `Standard` \| `Bulk`  
*Type*: String  
*Required*: no 

**Type**  
The job type\. You can initiate a job to perform a select query on an archive, retrieve an archive, or get an inventory of a vault\.  
*Valid Values*: `select` \| `archive-retrieval` \| `inventory-retrieval`   
*Type*: String  
*Required*: yes

## More Info<a name="more-info-api-jobParameters"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)