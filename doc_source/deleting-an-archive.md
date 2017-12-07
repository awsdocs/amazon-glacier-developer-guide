# Deleting an Archive in Amazon Glacier<a name="deleting-an-archive"></a>

You cannot delete an archive using the Amazon Glacier management console\. To delete an archive you must use the AWS Command Line Interface \(CLI\) or write code to make a delete request using either the REST API directly or the AWS SDK for Java and \.NET wrapper libraries\. For information on using the CLI with Amazon Glacier, see [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. The following topics explain how to use the AWS SDK for Java and \.NET wrapper libraries, and the REST API\.


+ [Deleting an Archive in Amazon Glacier Using the AWS SDK for Java](deleting-an-archive-using-java.md)
+ [Deleting an Archive in Amazon Glacier Using the AWS SDK for \.NET](deleting-an-archive-using-dot-net.md)
+ [Deleting an Archive Using the REST API](deleting-an-archive-using-rest.md)

You can delete one archive at a time from a vault\. To delete the archive you must provide its archive ID in your delete request\. You can get the archive ID by downloading the vault inventory for the vault that contains the archive\. For more information about downloading the vault inventory, see [Downloading a Vault Inventory in Amazon Glacier](vault-inventory.md)\. 

After you delete an archive, you might still be able to make a successful request to initiate a job to retrieve the deleted archive, but the archive retrieval job will fail\. 

Archive retrievals that are in progress for an archive ID when you delete the archive might or might not succeed according to the following scenarios:

+ If the archive retrieval job is actively preparing the data for download when Amazon Glacier receives the delete archive request, then the archival retrieval operation might fail\. 

+ If the archive retrieval job has successfully prepared the archive for download when Amazon Glacier receives the delete archive request, then you will be able to download the output\. 

For more information about archive retrieval, see [Downloading an Archive in Amazon Glacier](downloading-an-archive.md)\. 

This operation is idempotent\. Deleting an already\-deleted archive does not result in an error\. 

After you delete an archive, if you immediately download the vault inventory, it might include the deleted archive in the list because Amazon Glacier prepares vault inventory only about once a day\.