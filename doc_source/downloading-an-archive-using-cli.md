# Downloading an Archive in Amazon S3 Glacier Using the AWS CLI<a name="downloading-an-archive-using-cli"></a>

You can download archives in Amazon S3 Glacier \(S3 Glacier\) using the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Download an Archive Using the AWS CLI](#Downloading-Archives-CLI-Implementation)

## \(Prerequisite\) Setting Up the AWS CLI<a name="Creating-Vaults-CLI-Setup"></a>

1. Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*\. 

    [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 

   [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

1. Verify the setup by entering the following commands at the command prompt\. These commands don't provide credentials explicitly, so the credentials of the default profile are used\.
   + Try using the help command\.

     ```
     aws help
     ```
   + Use `aws s3 ls` to get a list of buckets on the configured account\.

     ```
     aws s3 ls
     ```
   + Use `aws configure list` to see the current configuration data\.

     ```
     aws configure list
     ```

## Example: Download an Archive Using the AWS CLI<a name="Downloading-Archives-CLI-Implementation"></a>
**Note**  
In order to download your archives you must know your archive ids\. Steps 1\-4 will retrieve your archive ids\. If you already know the archive ids you wish to download skip to step 5\.

1. Use the `initiate-job` command to start an inventory\-retrieval job\. The inventory report will list your archive ids\.

   ```
   aws glacier initiate-job --vault-name awsexamplevault --account-id 111122223333 --job-parameters "{\"Type\":\"inventory-retrieval\"}"
   ```

    Expected output:

   ```
   {
       "location": "/111122223333/vaults/awsexamplevault/jobs/*** jobid ***", 
       "jobId": "*** jobid ***"
   }
   ```

1. Use the `describe-job` command to check status of the previous `inventory-retrieval` job command\.

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

1. Wait for the job to complete\.

   You must wait until the job output is ready for you to download\. If you set a notification configuration on the vault or specified an Amazon Simple Notification Service \(Amazon SNS\) topic when you initiated the job, S3 Glacier sends a message to the topic after it completes the job\. 

   You can set notification configuration for specific events on the vault\. For more information, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\. S3 Glacier sends a message to the specified SNS topic anytime the specific event occurs\.

1. When it's complete, use the `get-job-output` command to download the retrieval job to the file `output.json`\. This file will contain your archive ids\. 

   ```
   aws glacier get-job-output --vault-name awsexamplevault --account-id 111122223333 --job-id *** jobid *** output.json
   ```

   This command produces a file with the following fields\.

   ```
   {
   "VaultARN":"arn:aws:glacier:region:111122223333:vaults/awsexamplevault",
   "InventoryDate":"*** job completion date ***",
   "ArchiveList":[
   {"ArchiveId":"*** archiveid ***",
   "ArchiveDescription":*** archive description (if set) ***,
   "CreationDate":"*** archive creation date ***",
   "Size":"*** archive size (in bytes) ***",
   "SHA256TreeHash":"*** archive hash ***"
   }
   {"ArchiveId":
   ...
   ]}
   ```

1. Use the `initiate-job` command to start the retrieval process each archive from a vault\. You will need to specify the job parameter as `archive-retrieval` as seen below\.

   ```
   aws glacier initiate-job --vault-name awsexamplevault --account-id 111122223333 --job-parameters "{\"Type\":\"archive-retrieval\"}" --archiveId *** archiveId ***
   ```

1. Wait for the `archive-retrieval` job to complete\. Use the `describe-job` command to check status of the previous command\.

   ```
   aws glacier describe-job --vault-name awsexamplevault --account-id 111122223333 --job-id *** jobid ***
   ```

1. When the above job is complete use the `get-job-output` command to download your archive\.

   ```
   aws glacier get-job-output --vault-name awsexamplevault --account-id 111122223333 --job-id *** jobid *** output_file_name
   ```