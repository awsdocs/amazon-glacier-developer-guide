# Get Amazon S3 Glacier archive content and delete the archive using an AWS SDK<a name="example_glacier_Usage_RetrieveDelete_section"></a>

The following code example shows how to:
+ List jobs for an Amazon S3 Glacier vault and get job status\.
+ Get the output of a completed archive retrieval job\.
+ Delete an archive\.
+ Delete a vault\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/glacier#code-examples)\. 
Create a class that wraps S3 Glacier operations\.  

```
import argparse
import logging
import os
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger(__name__)


class GlacierWrapper:
    """Encapsulates Amazon S3 Glacier API operations."""
    def __init__(self, glacier_resource):
        """
        :param glacier_resource: A Boto3 Amazon S3 Glacier resource.
        """
        self.glacier_resource = glacier_resource


    @staticmethod
    def list_jobs(vault, job_type):
        """
        Lists jobs by type for the specified vault.

        :param vault: The vault to query.
        :param job_type: The type of job to list.
        :return: The list of jobs of the requested type.
        """
        job_list = []
        try:
            if job_type == 'all':
                jobs = vault.jobs.all()
            elif job_type == 'in_progress':
                jobs = vault.jobs_in_progress.all()
            elif job_type == 'completed':
                jobs = vault.completed_jobs.all()
            elif job_type == 'succeeded':
                jobs = vault.succeeded_jobs.all()
            elif job_type == 'failed':
                jobs = vault.failed_jobs.all()
            else:
                jobs = []
                logger.warning("%s isn't a type of job I can get.", job_type)
            for job in jobs:
                job_list.append(job)
                logger.info("Got %s %s job %s.", job_type, job.action, job.id)
        except ClientError:
            logger.exception("Couldn't get %s jobs from %s.", job_type, vault.name)
            raise
        else:
            return job_list

    @staticmethod
    def get_job_output(job):
        """
        Gets the output of a job, such as a vault inventory or the contents of an
        archive.

        :param job: The job to get output from.
        :return: The job output, in bytes.
        """
        try:
            response = job.get_output()
            out_bytes = response['body'].read()
            logger.info("Read %s bytes from job %s.", len(out_bytes), job.id)
            if 'archiveDescription' in response:
                logger.info(
                    "These bytes are described as '%s'", response['archiveDescription'])
        except ClientError:
            logger.exception("Couldn't get output for job %s.", job.id)
            raise
        else:
            return out_bytes

    @staticmethod
    def delete_archive(archive):
        """
        Deletes an archive from a vault.

        :param archive: The archive to delete.
        """
        try:
            archive.delete()
            logger.info(
                "Deleted archive %s from vault %s.", archive.id, archive.vault_name)
        except ClientError:
            logger.exception("Couldn't delete archive %s.", archive.id)
            raise

    @staticmethod
    def delete_vault(vault):
        """
        Deletes a vault.

        :param vault: The vault to delete.
        """
        try:
            vault.delete()
            logger.info("Deleted vault %s.", vault.name)
        except ClientError:
            logger.exception("Couldn't delete vault %s.", vault.name)
            raise
```
Call functions on the wrapper class to get archive content from a completed job, then delete the archive\.  

```
def retrieve_demo(glacier, vault_name):
    """
    Shows how to:
    * List jobs for a vault and get job status.
    * Get the output of a completed archive retrieval job.
    * Delete an archive.
    * Delete a vault.

    :param glacier: A Boto3 Amazon S3 Glacier resource.
    :param vault_name: The name of the vault to query for jobs.
    """
    vault = glacier.glacier_resource.Vault('-', vault_name)
    try:
        vault.load()
    except ClientError as err:
        if err.response['Error']['Code'] == 'ResourceNotFoundException':
            print(f"\nVault {vault_name} doesn't exist. You must first run this script "
                  f"with the --upload flag to create the vault.")
            return
        else:
            raise

    print(f"\nGetting completed jobs for {vault.name}.")
    jobs = glacier.list_jobs(vault, 'completed')
    if not jobs:
        print("\nNo completed jobs found. Give it some time and try again later.")
        return

    retrieval_job = None
    for job in jobs:
        if job.action == 'ArchiveRetrieval' and job.status_code == 'Succeeded':
            retrieval_job = job
            break
    if retrieval_job is None:
        print("\nNo ArchiveRetrieval jobs found. Give it some time and try again "
              "later.")
        return

    print(f"\nGetting output from job {retrieval_job.id}.")
    archive_bytes = glacier.get_job_output(retrieval_job)
    archive_str = archive_bytes.decode('utf-8')
    print("\nGot archive data. Printing the first 10 lines.")
    print(os.linesep.join(archive_str.split(os.linesep)[:10]))

    print(f"\nDeleting the archive from {vault.name}.")
    archive = glacier.glacier_resource.Archive(
        '-', vault.name, retrieval_job.archive_id)
    glacier.delete_archive(archive)

    print(f"\nDeleting {vault.name}.")
    glacier.delete_vault(vault)
```
+ For API details, see the following topics in *AWS SDK for Python \(Boto3\) API Reference*\.
  + [DeleteArchive](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/DeleteArchive)
  + [DeleteVault](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/DeleteVault)
  + [GetJobOutput](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/GetJobOutput)
  + [ListJobs](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/ListJobs)

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.