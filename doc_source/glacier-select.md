# Querying Archives with Amazon Glacier Select<a name="glacier-select"></a>

With Amazon Glacier Select, you can perform filtering operations using simple Structured Query Language \(SQL\) statements directly on your data in Amazon Glacier\. When you provide an SQL query for an Amazon Glacier archive object, Amazon Glacier Select runs the query in place and writes the output results to Amazon S3\. With Amazon Glacier Select, you can run queries and custom analytics on your data that is stored in Amazon Glacier, without having to restore your data to a hotter tier like Amazon S3\.

When you perform select queries, Amazon Glacier provides three data access tiers—*expedited*, *standard*, and *bulk*\. All of these tiers provide different data access times and costs, and you can choose any one of them depending on how quickly you want your data to be available\. For all but the largest archives \(250 MB\+\), data that is accessed using the expedited tier is typically made available within 1–5 minutes\. The standard tier finishes within 3–5 hours\. The bulk retrievals finish within 5–12 hours\. For information about tier pricing, see [Amazon Glacier Pricing](http://aws.amazon.com/glacier/pricing/)\.

You can use Amazon Glacier Select with the AWS SDKs, the Amazon Glacier REST API, and the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [Amazon Glacier Select Requirements and Limits](#glacier-select-requirements-and-limits)
+ [How Do I Query Data Using Amazon Glacier Select?](#glacier-select-restrictions)
+ [Error Handling](#glacier-select-access-control)
+ [More Info](#related-sections-glacier-select)

## Amazon Glacier Select Requirements and Limits<a name="glacier-select-requirements-and-limits"></a>

The following are requirements for using Amazon Glacier Select:
+ Archive objects that are queried by Amazon Glacier Select must be formatted as uncompressed comma\-separated values \(CSV\)\. 
+ You must have an S3 bucket to work with\. In addition, the AWS account that you use to initiate an Amazon Glacier Select job must have write permissions for the S3 bucket\. The Amazon S3 bucket must be in the same AWS Region as the vault that contains the archive object that is being queried\. 
+ You must have permission to call [Get Job Output \(GET output\)](api-job-output-get.md)\.

The following limits apply when using Amazon Glacier Select:
+ There are no limits on the number of records that Amazon Glacier Select can process\. An input or output record must not exceed 1 MB; otherwise, the query fails\. There is a limit of 1,048,576 columns per record\.
+ There is no limit on the size of your final result\. However, your results are broken into multiple parts\. 
+ An SQL expression is limited to 128 KB\.

## How Do I Query Data Using Amazon Glacier Select?<a name="glacier-select-restrictions"></a>

Using Amazon Glacier Select, you can use SQL commands to query Amazon Glacier archive objects that are in uncompressed CSV format\. With this restriction, you can perform simple query operations on your text\-based data in Amazon Glacier\. For example, you might look for a specific name or ID among a set of archive text files\. 

To query your Amazon Glacier data, create a select *job* using the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) operation\. When initiating a select job, you provide the SQL expression, the archive to query, and the location to store the results in Amazon S3\. 

The following example expression returns all records from the archive specified by the archive ID in [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.

```
SELECT * FROM archive
```

Amazon Glacier Select supports a subset of the ANSI SQL language\. It supports common filtering SQL clauses like SELECT, FROM, and WHERE\. It does not support SUM, COUNT, GROUP BY, JOINS, DISTINCT, UNION, ORDER BY, and LIMIT\. For more information about support for SQL, see [SQL Reference for Amazon S3 Select and Amazon Glacier Select](s3-glacier-select-sql-reference.md)\.

### Amazon Glacier Select Output<a name="glacier-select-output"></a>

When you initiate a select job, you define an output location for the results of your select query\. This location must be an Amazon S3 bucket in the same AWS Region as the vault that contains the archive object that is being queried\. The AWS account that initiates the job must have permissions to write to the S3 bucket\. 

You can specify the S3 storage class and encryption for the output objects stored in Amazon S3\. Amazon Glacier Select supports SSE\-KMS and SSE\-S3 encryption\. Amazon Glacier Select doesn't support SSE\-C and client\-side encryption\. For more information about Amazon S3 storage classes and encryption, see [Storage Classes](http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) and [Protecting Data Using Server\-Side Encryption](http://docs.aws.amazon.com/AmazonS3/latest/dev/serv-side-encryption.html) in the *Amazon Simple Storage Service Developer Guide*\.

Amazon Glacier Select results are stored in the S3 bucket using the prefix provided in the output location specified in [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\. From this information, Amazon Glacier Select creates a unique prefix referring to the job ID\. This job ID prefix is returned in the `x-amz-job-output-path` header in an [Initiate Job \(POST jobs\)](api-initiate-job-post.md) response\. \(Prefixes are used to group S3 objects together by beginning object names with a common string\.\) Under this unique prefix, there are two new prefixes created, `results` for results and `errors` for logs and errors\. Upon completion of the job, a result manifest is written which contains the location of all results\.

There is also a placeholder file named `job.txt` that is written to the output location\. After it is written it is never updated\. The placeholder file is used for the following:
+ Validation of the write permission and majority of SQL syntax errors synchronously\. 
+ Provides a static output similar to [Describe Job \(GET JobID\)](api-describe-job-get.md) that you can easily reference whenever you want\. 

For example, suppose that you initiate an Amazon Glacier Select job with the output location for the results specified as `s3://example-bucket/my-prefix`, and the job response returns the job ID as `examplekne1209ualkdjh812elkassdu9012e`\. After the select job finishes, you can see the following Amazon S3 objects in your bucket:

```
s3://example-bucket/my-prefix/examplekne1209ualkdjh812elkassdu9012e/job.txt
s3://example-bucket/my-prefix/examplekne1209ualkdjh812elkassdu9012e/results/abc
s3://example-bucket/my-prefix/examplekne1209ualkdjh812elkassdu9012e/results/def
s3://example-bucket/my-prefix/examplekne1209ualkdjh812elkassdu9012e/results/ghi
s3://example-bucket/my-prefix/examplekne1209ualkdjh812elkassdu9012e/result_manifest.txt
```

The select query results are broken into multiple parts\. In the example, Amazon Glacier Select uses the prefix that you specified when setting the output location and appends the job ID and the `results` prefix\. It then writes the results in three parts, with the object names ending in `abc`, `def`, and `ghi`\. The result manifest contains all the three files to allow programmatic retrieval\. If the job fails with any error, then a file is visible under the error prefix and an `error_manifest.txt` is produced\.

Presence of a `result_manifest.txt` file along with the absence of `error_manifest.txt` guarantees that the job finished successfully\. There is no guarantee provided on how results are ordered\.

**Note**  
The length of an Amazon S3 object name, also referred to as the *key*, can be no more than 1024 bytes\. Amazon Glacier reserves 128 bytes for prefixes\. And, the length of your Amazon S3 location path cannot be more than 512 bytes\. A request with a length greater than 512 bytes returns an exception, and the request is not accepted\.

## Error Handling<a name="glacier-select-access-control"></a>

Amazon Glacier Select notifies you of two kinds of errors\. The first set of errors is sent to you synchronously when you submit the query in [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\. These errors are sent to you as part of the HTTP response\. Another set of errors can occur after the query has been accepted successfully, but they happen during query execution\. In this case, the errors are written to the specified output location under the `errors` prefix\.

Amazon Glacier Select will stop executing the query after encountering an error\. To execute the query successfully, you must resolve all errors\. You can check the logs to identify which records caused a failure\. 

Because queries run in parallel across multiple compute nodes, the errors that you get are not in sequential order\. For example, if your query fails with an error in row 6234, it does not mean that all rows before row 6234 were successfully processed\. The next run of the query might show an error in a different row\. 

## More Info<a name="related-sections-glacier-select"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)
+ [Describe Job \(GET JobID\)](api-describe-job-get.md)
+ [List Jobs \(GET jobs\)](api-jobs-get.md)
+ [Working with Archives in Amazon Glacier](working-with-archives.md)