# Amazon S3 Glacier Data Model<a name="amazon-glacier-data-model"></a>

The Amazon S3 Glacier \(S3 Glacier\) data model core concepts include vaults and archives\. S3 Glacier is a REST\-based web service\. In terms of REST, vaults and archives are the resources\. In addition, the S3 Glacier data model includes job and notification\-configuration resources\. These resources complement the core resources\.

**Topics**
+ [Vault](#data-model-vault)
+ [Archive](#data-model-archive)
+ [Job](#data-model-job)
+ [Notification Configuration](#data-model-notification-config)

## Vault<a name="data-model-vault"></a>

In S3 Glacier, a vault is a container for storing archives\. When you create a vault, you specify a name and choose an AWS Region where you want to create the vault\.

Each vault resource has a unique address\. The general form is:

```
https://<region-specific endpoint>/<account-id>/vaults/<vaultname>
```

For example, suppose that you create a vault \(`examplevault`\) in the US West \(Oregon\) Region\. This vault can then be addressed by the following URI:

```
https://glacier.us-west-2.amazonaws.com/111122223333/vaults/examplevault
```

In the URI, 
+ `glacier.us-west-2.amazonaws.com` identifies the US West \(Oregon\) Region\. 
+ `111122223333` is the AWS account ID that owns the vault\.
+ `vaults` refers to the collection of vaults owned by the AWS account\.
+ `examplevault` identifies a specific vault in the vaults collection\.

An AWS account can create vaults in any supported AWS Region\. For list of supported AWS Regions, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\. Within a Region, an account must use unique vault names\. An AWS account can create same\-named vaults in different Regions\.

You can store an unlimited number of archives in a vault\. Depending on your business or application needs, you can store these archives in one vault or multiple vaults\. 

S3 Glacier supports various vault operations\. Note that vault operations are Region specific\. For example, when you create a vault, you create it in a specific Region\. When you request a vault list, you request it from a specific AWS Region, and the resulting list only includes vaults created in that specific Region\.

## Archive<a name="data-model-archive"></a>

An archive can be any data such as a photo, video, or document and is a base unit of storage in S3 Glacier\. Each archive has a unique ID and an optional description\. Note that you can only specify the optional description during the upload of an archive\. S3 Glacier assigns the archive an ID, which is unique in the AWS Region in which it is stored\. 

Each archive has a unique address\. The general form is as follows:

```
https://<region-specific endpoint>/<account-id>/vaults/<vault-name>/archives/<archive-id>
```

The following is an example URI of an archive stored in the `examplevault` vault in the US West \(Oregon\) Region:

```
https://glacier.us-west-2.amazonaws.com/111122223333/vaults/examplevault/archives/NkbByEejwEggmBz2fTHgJrg0XBoDfjP4q6iu87-TjhqG6eGoOY9Z8i1_AUyUsuhPAdTqLHy8pTl5nfCFJmDl2yEZONi5L26Omw12vcs01MNGntHEQL8MBfGlqrEXAMPLEArchiveId
```

You can store an unlimited number of archives in a vault\.

In addition, the S3 Glacier data model includes job and notification\-configuration resources\. These resources complement the core vault and archive resources\.

## Job<a name="data-model-job"></a>

S3 Glacier jobs can perform a select query on an archive, retrieve an archive, or get an inventory of a vault\. When performing a query on an archive, you initiate a job providing a SQL query and list of S3 Glacier archive objects\. S3 Glacier Select runs the query in place and writes the output results to Amazon S3\.

Retrieving an archive and vault inventory \(list of archives\) are asynchronous operations in S3 Glacier in which you first initiate a job, and then download the job output after S3 Glacier completes the job\. 

**Note**  
S3 Glacier offers a cold storage data archival solution\. If your application needs a storage solution that requires real\-time data retrieval, you might consider using Amazon S3\. For more information, see [Amazon Simple Storage Service \(Amazon S3\)](http://aws.amazon.com/s3)\.

To initiate a vault inventory job, you provide a vault name\. Select and archive retrieval jobs require that both the vault name and the archive ID\. You can also provide an optional job description to help identify the jobs\. 

Select, archive retrieval, and vault inventory jobs are associated with a vault\. A vault can have multiple jobs in progress at any point in time\. When you send a job request \(initiate a job\), S3 Glacier returns to you a job ID to track the job\. Each job is uniquely identified by a URI of the form:

```
https://<region-specific endpoint>/<account-id>/vaults/<vault-name>/jobs/<job-id>
```

The following is an example of a job associated with an `examplevault` vault\.

```
https://glacier.us-west-2.amazonaws.com/111122223333/vaults/examplevault/jobs/HkF9p6o7yjhFx-K3CGl6fuSm6VzW9T7esGQfco8nUXVYwS0jlb5gq1JZ55yHgt5vP54ZShjoQzQVVh7vEXAMPLEjobID
```

For each job, S3 Glacier maintains information such as job type, description, creation date, completion date, and job status\. You can obtain information about a specific job or obtain a list of all your jobs associated with a vault\. The list of jobs that S3 Glacier returns includes all the in\-progress and recently finished jobs\. 

## Notification Configuration<a name="data-model-notification-config"></a>

Because jobs take time to complete, S3 Glacier supports a notification mechanism to notify you when a job is complete\. You can configure a vault to send notification to an Amazon Simple Notification Service \(Amazon SNS\) topic when jobs complete\. You can specify one SNS topic per vault in the notification configuration\.

S3 Glacier stores the notification configuration as a JSON document\. The following is an example vault notification configuration:

```
{
   "Topic": "arn:aws:sns:us-west-2:111122223333:mytopic", 
   "Events": ["ArchiveRetrievalCompleted", "InventoryRetrievalCompleted"] 
}
```

Note that notification configurations are associated with vaults; you can have one for each vault\. Each notification configuration resource is uniquely identified by a URI of the form:

```
https://<region-specific endpoint>/<account-id>/vaults/<vault-name>/notification-configuration
```

S3 Glacier supports operations to set, get, and delete a notification configuration\. When you delete a notification configuration, no notifications are sent when any data retrieval operation on the vault is complete\.