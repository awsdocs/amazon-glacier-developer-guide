# Supported Operations in Amazon Glacier<a name="amazon-glacier-supported-operations"></a>

To work with vaults and archives \(see [Amazon Glacier Data Model](amazon-glacier-data-model.md)\), Amazon Glacier supports a set of operations\. Among all the supported operations, only the following operations are asynchronous:

+ Retrieving an archive

+ Retrieving a vault inventory \(list of archives\)

These operations require you to first initiate a job and then download the job output\. The following sections summarize the Amazon Glacier operations:

## Vault Operations<a name="vault-ops-intro"></a>

Amazon Glacier provides operations to create and delete vaults\. You can obtain a vault description for a specific vault or for all vaults in a region\. The vault description provides information such as creation date, number of archives in the vault, total size in bytes used by all the archives in the vault, and the date Amazon Glacier generated the vault inventory\. Amazon Glacier also provides operations to set, retrieve, and delete a notification configuration on the vault\. For more information, see [Working with Vaults in Amazon Glacier](working-with-vaults.md)\.

## Archive Operations<a name="archive-ops-intro"></a>

Amazon Glacier provides operations for you to upload and delete archives\. You cannot update an existing archive; you must delete the existing archive and upload a new archive\. Note that each time you upload an archive, Amazon Glacier generates a new archive ID\. For more information, see [Working with Archives in Amazon Glacier](working-with-archives.md)\.

## Jobs<a name="job-ops-intro"></a>

You can initiate an Amazon Glacier job to perform a select query on an archive, retrieve an archive, or get an inventory of a vault\.

The following are the types of Amazon Glacier jobs: 

+ `select`— Perform a select query on an archive\.

  For more information, see [Querying Archives with Amazon Glacier Select](glacier-select.md)\.

+ `archive-retrieval`— Retrieve an archive\. 

  For more information, see [Downloading an Archive in Amazon Glacier](downloading-an-archive.md)\.

+ `inventory-retrieval`— Inventory a vault\.

  For more information, see [Downloading a Vault Inventory in Amazon Glacier](vault-inventory.md)\.