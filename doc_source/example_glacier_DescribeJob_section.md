# Describe an Amazon S3 Glacier job using an AWS SDK<a name="example_glacier_DescribeJob_section"></a>

The following code examples show how to describe an Amazon S3 Glacier job\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Glacier#code-examples)\. 
  

```
    using System;
    using System.Threading.Tasks;
    using Amazon.Glacier;
    using Amazon.Glacier.Model;

    public class DescribeVault
    {
        public static async Task Main(string[] args)
        {
            string vaultName = "example-vault";
            var client = new AmazonGlacierClient();
            var request = new DescribeVaultRequest
            {
                AccountId = "-",
                VaultName = vaultName,
            };

            var response = await client.DescribeVaultAsync(request);

            // Display the information about the vault.
            Console.WriteLine($"{response.VaultName}\tARN: {response.VaultARN}");
            Console.WriteLine($"Created on: {response.CreationDate}\tNumber of Archives: {response.NumberOfArchives}\tSize (in bytes): {response.SizeInBytes}");
            if (response.LastInventoryDate != DateTime.MinValue)
            {
                Console.WriteLine($"Last inventory: {response.LastInventoryDate}");
            }
        }
    }
```
+  For API details, see [DescribeJob](https://docs.aws.amazon.com/goto/DotNetSDKV3/glacier-2012-06-01/DescribeJob) in *AWS SDK for \.NET API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/glacier#code-examples)\. 
  

```
class GlacierWrapper:
    """Encapsulates Amazon S3 Glacier API operations."""
    def __init__(self, glacier_resource):
        """
        :param glacier_resource: A Boto3 Amazon S3 Glacier resource.
        """
        self.glacier_resource = glacier_resource


    @staticmethod
    def get_job_status(job):
        """
        Gets the status of a job.

        :param job: The job to query.
        :return: The current status of the job.
        """
        try:
            job.load()
            logger.info(
                "Job %s is performing action %s and has status %s.", job.id,
                job.action, job.status_code)
        except ClientError:
            logger.exception("Couldn't get status for job %s.", job.id)
            raise
        else:
            return job.status_code
```
+  For API details, see [DescribeJob](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/DescribeJob) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.