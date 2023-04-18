# Delete an Archive in S3 Glacier by Using the AWS CLI<a name="getting-started-delete-archive-cli"></a>

You can delete archives in Amazon S3 Glacier by using the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Deleting an Archive by Using the AWS CLI](#getting-started-Deleting-Archives-CLI-Implementation)

## \(Prerequisite\) Setting Up the AWS CLI<a name="Creating-Vaults-CLI-Setup"></a>

1. Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*: 

    [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 

   [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

1. Verify your AWS CLI setup by entering the following commands at the command prompt\. These commands don't provide credentials explicitly, so the credentials of the default profile are used\.
   + Try using the help command\.

     ```
     aws help
     ```
   + To get a list of S3 Glacier vaults on the configured account, use the `list-vaults` command\. Replace *123456789012* with your AWS account ID\.

     ```
     aws glacier list-vaults --account-id 123456789012
     ```
   + To see the current configuration data for the AWS CLI, use the `aws configure list` command\.

     ```
     aws configure list
     ```

## Example: Deleting an Archive by Using the AWS CLI<a name="getting-started-Deleting-Archives-CLI-Implementation"></a>

1. Use the `initiate-job` command to start an inventory retrieval job\. For more information on the `initiate-job` command, see [Initiate Job](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-initiate-job-post.html)\.

   ```
   aws glacier initiate-job --vault-name awsexamplevault --account-id 111122223333 --job-parameters '{"Type": "inventory-retrieval"}'
   ```

    Expected output:

   ```
   {
       "location": "/111122223333/vaults/awsexamplevault/jobs/*** jobid ***", 
       "jobId": "*** jobid ***"
   }
   ```

1. Use the `describe-job` command to check the status of the previous retrieval job\. For more information on the `describe-job` command, see [Describe Job](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-describe-job-get.html)\.

   ```
   aws glacier describe-job --vault-name awsexamplevault --account-id 111122223333 --job-id *** jobid ***
   ```

    Expected output:

   ```
   {
       "InventoryRetrievalParameters": {
           "Format": "JSON"
       }, 
       "VaultARN": "*** vault arn ***", 
       "Completed": false, 
       "JobId": "*** jobid ***", 
       "Action": "InventoryRetrieval", 
       "CreationDate": "*** job creation date ***", 
       "StatusCode": "InProgress"
   }
   ```

1. Wait for the job to be completed\.

   You must wait until the job output is ready for you to download\. If you set a notification configuration on the vault or specified an Amazon Simple Notification Service \(Amazon SNS\) topic when you initiated the job, S3 Glacier sends a message to the topic after it completes the job\. 

   You can set notification configuration for specific events on the vault\. For more information, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\. S3 Glacier sends a message to the specified Amazon SNS topic anytime the specific event occurs\.

1. When the job is complete, use the `get-job-output` command to download the retrieval job to the file `output.json`\. For more information on the `get-job-output` command, see [Get Job Output](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-job-output-get.html)\.

   ```
   aws glacier get-job-output --vault-name awsexamplevault --account-id 111122223333 --job-id *** jobid *** output.json
   ```

   This command produces a file with the following fields\.

   ```
   {
   "VaultARN":"arn:aws:glacier:region:111122223333:vaults/awsexamplevault",
   "InventoryDate":""*** job completion date ***"",
   "ArchiveList":[{
   {"ArchiveId":""*** archiveid ***"",
   "ArchiveDescription":"*** archive description (if set) ***",
   "CreationDate":""*** archive creation date ***"",
   "Size":""*** archive size (in bytes) ***"",
   "SHA256TreeHash":"*** archive hash ***"
   }],
   "ArchiveId": 123456789
   
   }
   ```

1. Use the `delete-archive` command to delete each archive from a vault until none remain\.

   ```
   aws glacier delete-archive --vault-name awsexamplevault --account-id 111122223333 --archive-id="*** archiveid ***"
   ```

 For more information on the `delete-archive` command, see [Delete Archive](https://docs.aws.amazon.com/amazonglacier/latest/dev/api-archive-delete.html)\.