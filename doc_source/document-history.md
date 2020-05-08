# Document History<a name="document-history"></a>
+ **Latest documentation update:** November 20, 2018
+ **Current product version:** 2012\-06\-01

The following table describes the important changes in each release of the *Amazon S3 Glacier Developer Guide* from July 5, 2018, onward\. For notification about updates to this documentation, you can subscribe to an RSS feed\.

| Change | Description | Date | 
| --- |--- |--- |
| [Amazon Glacier name change](#document-history) | Amazon Glacier is now Amazon S3 Glacier to better reflect Glacier's integration with Amazon S3\. | November 20, 2018 | 
| [Updates now available over RSS](#document-history) | You can now subscribe to an RSS feed to receive notifications about updates to the Amazon S3 Glacier Developer Guide guide\. | July 5, 2018 | 

## Earlier Updates<a name="document-history-earlier"></a>

The following table describes the important changes in each release of the *Amazon S3 Glacier Developer Guide* before July 5, 2018\.


| Change | Description | Release Date | 
| --- | --- | --- | 
|  Querying archives with SQL  |  S3 Glacier now supports querying data archives with SQL\. For more information, see [Querying Archives with S3 Glacier Select](glacier-select.md)\. The following APIs are updated accordingly:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/document-history.html)  |  November 29, 2017  | 
|  Expedited and Bulk Data Retrievals  |  S3 Glacier now supports Expedited and Bulk data retrievals in addition to Standard retrievals\. For more information, see [Archive Retrieval Options](downloading-an-archive-two-steps.md#api-downloading-an-archive-two-steps-retrieval-options)\.   |  November 21, 2016  | 
|  Vault Lock  |  S3 Glacier now supports Vault Lock, which allows you to easily deploy and enforce compliance controls on individual S3 Glacier vaults with a Vault Lock policy\. For more information, see [Amazon S3 Glacier Vault Lock](vault-lock.md) and [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.   |  July 8, 2015  | 
|  Vault tagging  |  S3 Glacier now allows you to tag your S3 Glacier vaults for easier resource and cost management\. Tags are labels that you can define and associate with your vaults, and using tags adds filtering capabilities to operations such as AWS cost reports\. For more information, see [Tagging Amazon S3 Glacier Resources](tagging.md) and [Tagging Your Amazon S3 Glacier Vaults](tagging-vaults.md)\.  |  June 22, 2015  | 
|  Vault access policies  |  S3 Glacier now supports managing access to your individual S3 Glacier vaults by using vault access policies\. You can now define an access policy directly on a vault, making it easier to grant vault access to users and business groups internal to your organization, as well as to your external business partners\. For more information, see [Amazon S3 Glacier Access Control with Vault Access Policies](vault-access-policy.md)\.  |  April 27, 2015  | 
|  Data retrieval policies and audit logging  |  S3 Glacier now supports data retrieval policies and audit logging\. Data retrieval policies allow you to easily set data retrieval limits and simplify data retrieval cost management\. You can define your own data retrieval limits with a few clicks in the AWS console or by using the S3 Glacier API\. For more information, see [Amazon S3 Glacier Data Retrieval Policies](data-retrieval-policy.md)\. In addition, S3 Glacier now supports audit logging with AWS CloudTrail, which records S3 Glacier API calls for your account and delivers the log files to an Amazon S3 bucket that you specify\. For more information, see [Logging Amazon S3 Glacier API Calls with AWS CloudTrail](audit-logging.md)\.  |  December 11, 2014  | 
|  Updates to Java samples  |  Updated the Java code samples in this guide that use the AWS SDK for Java\.  |  June 27, 2014  | 
|  Limiting vault inventory retrieval  |  You can now limit the number of vault inventory items retrieved by filtering on the archive creation date or by setting a limit\. For more information about limiting inventory retrieval, see [Range Inventory Retrieval](api-initiate-job-post.md#api-initiate-job-post-vault-inventory-list-filtering) in the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) topic\.  |  December 31, 2013  | 
|  Removed outdated URLs  |  Removed the URLs that pointed to the old security credentials page from code examples\.  |  July 26, 2013  | 
|  Support for range retrievals  |  S3 Glacier now supports retrieval of specific ranges of your archives\. You can initiate a job requesting S3 Glacier to prepare an entire archive or a portion of the archive for subsequent download\. When an archive is very large, you may find it cost effective to initiate several sequential jobs to prepare your archive\.  For more information, see [Downloading an Archive in Amazon S3 Glacier](downloading-an-archive.md)\. For pricing information, go to the [S3 Glacier detail page](http://aws.amazon.com/glacier)\.   |  November 13, 2012  | 
|  New Guide  |  This is the first release of the *Amazon S3 Glacier Developer Guide*\.   |  August 20, 2012  | 