# Deleting an Archive in Amazon S3 Glacier<a name="deleting-an-archive"></a>

You cannot delete an archive using the Amazon S3 Glacier \(S3 Glacier\) management console\. To delete an archive you must use the AWS Command Line Interface \(CLI\) or write code to make a delete request using either the REST API directly or the AWS SDK for Java and \.NET wrapper libraries\. The following topics explain how to use the AWS SDK for Java and \.NET wrapper libraries, the REST API, and the AWS CLI\.

**Topics**
+ [Deleting an Archive in Amazon S3 Glacier Using the AWS SDK for Java](deleting-an-archive-using-java.md)
+ [Deleting an Archive in Amazon S3 Glacier Using the AWS SDK for \.NET](deleting-an-archive-using-dot-net.md)
+ [Deleting an Amazon S3 Glacier Archive Using the REST API](deleting-an-archive-using-rest.md)
+ [Deleting an Archive in Amazon S3 Glacier Using the AWS Command Line Interface](deleting-an-archive-using-cli.md)

You can delete one archive at a time from a vault\. To delete the archive you must provide its archive ID in your delete request\. You can get the archive ID by downloading the vault inventory for the vault that contains the archive\. For more information about downloading the vault inventory, see [Downloading a Vault Inventory in Amazon S3 Glacier](vault-inventory.md)\. 

After you delete an archive, you might still be able to make a successful request to initiate a job to retrieve the deleted archive, but the archive retrieval job will fail\. 

Archive retrievals that are in progress for an archive ID when you delete the archive might or might not succeed according to the following scenarios:
+ If the archive retrieval job is actively preparing the data for download when S3 Glacier receives the delete archive request, then the archival retrieval operation might fail\. 
+ If the archive retrieval job has successfully prepared the archive for download when S3 Glacier receives the delete archive request, then you will be able to download the output\. 

For more information about archive retrieval, see [Downloading an Archive in Amazon S3 Glacier](downloading-an-archive.md)\. 

This operation is idempotent\. Deleting an already\-deleted archive does not result in an error\. 

After you delete an archive, if you immediately download the vault inventory, it might include the deleted archive in the list because S3 Glacier prepares vault inventory only about once a day\.