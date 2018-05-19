# InventoryRetrievalJobInput<a name="api-InventoryRetrievalJobInput"></a>

 Provides options for specifying a range inventory retrieval job\.

## Contents<a name="api-InventoryRetrievalJobInput-contents"></a>

**EndDate**  
The end of the date range, in UTC time, for a vault inventory retrieval that includes archives created before this date\.  
*Valid Values*: A string representation in the ISO 8601 date format \(`YYYY-MM-DDThh:mm:ssTZD`\) in seconds, for example `2013-03-20T17:03:43Z`\.  
*Type*: String\. A string representation in the ISO 8601 date format \(`YYYY-MM-DDThh:mm:ssTZD`\) in seconds, for example `2013-03-20T17:03:43Z`\.  
*Required*: no

**Format**  
 The output format for the vault inventory list, which is set by the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) request when initiating a job to retrieve a vault inventory\.  
*Valid Values*: `CSV` \| `JSON`   
*Required*: no  
*Type*: String

**Limit**  
 The maximum number of inventory items that can be returned for each vault inventory retrieval request\.  
*Valid Values*: An integer value greater than or equal to 1\.  
*Type*: String  
*Required*: no

**Marker**  
 An opaque string that represents where to continue pagination of the vault inventory retrieval results\. You use this marker in a new `Initiate Job` request to obtain additional inventory items\. If there are no more inventory items, this value is null\.   
*Type*: String  
*Required*: no

**StartDate**  
The start of the date range, in UTC time, for a vault inventory retrieval that includes archives created on or after this date\.  
*Valid Values*: A string representation in the ISO 8601 date format \(`YYYY-MM-DDThh:mm:ssTZD`\) in seconds, for example `2013-03-20T17:03:43Z`\.   
*Type*: String\. A string representation in the ISO 8601 date format \(`YYYY-MM-DDThh:mm:ssTZD`\) in seconds, for example `2013-03-20T17:03:43Z`\.  
*Required*: no

## More Info<a name="more-info-api-InventoryRetrievalJobInput"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)