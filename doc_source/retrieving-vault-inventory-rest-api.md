# Downloading a Vault Inventory Using the REST API<a name="retrieving-vault-inventory-rest-api"></a>

**To download a vault inventory using the REST API**

Downloading a vault inventory is a two\-step process\.

1. Initiate a job of the `inventory-retrieval` type\. For more information, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.

1. After the job completes, download the inventory data\. For more information, see [Get Job Output \(GET output\)](api-job-output-get.md)\.