# Retrieve an Amazon S3 Glacier vault inventory using an AWS SDK<a name="example_glacier_InitiateJob_InventoryRetrieval_section"></a>

The following code examples show how to retrieve an Amazon S3 Glacier vault inventory\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

Action examples are code excerpts from larger programs and must be run in context\. You can see this action in context in the following code examples: 
+  [Retrieve an archive from a vault](example_glacier_InitiateJob_ArchiveRetrieval_section.md) 
+  [Archive a file, get notifications, and initiate a job](example_glacier_Usage_UploadNotifyInitiate_section.md) 

------
#### [ Java ]

**SDK for Java 2\.x**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javav2/example_code/glacier#readme)\. 
  

```
    public static String createJob(GlacierClient glacier, String vaultName, String accountId) {

        try {

            JobParameters job = JobParameters.builder()
                .type("inventory-retrieval")
                .build();

            InitiateJobRequest initJob = InitiateJobRequest.builder()
                .jobParameters(job)
                .accountId(accountId)
                .vaultName(vaultName)
                .build();

            InitiateJobResponse response = glacier.initiateJob(initJob);
            System.out.println("The job ID is: " +response.jobId()) ;
            System.out.println("The relative URI path of the job is: " +response.location()) ;
            return response.jobId();

        } catch(GlacierException e) {
            System.err.println(e.awsErrorDetails().errorMessage());
            System.exit(1);

        }
        return "";
    }

    //  Poll S3 Glacier = Polling a Job may take 4-6 hours according to the Documentation.
    public static void checkJob(GlacierClient glacier, String jobId, String name, String account, String path) {

       try{
           boolean finished = false;
           String jobStatus;
           int yy=0;

           while (!finished) {
               DescribeJobRequest jobRequest = DescribeJobRequest.builder()
                   .jobId(jobId)
                   .accountId(account)
                   .vaultName(name)
                   .build();

                DescribeJobResponse response = glacier.describeJob(jobRequest);
                jobStatus = response.statusCodeAsString();

               if (jobStatus.compareTo("Succeeded") == 0)
                   finished = true;
               else {
                   System.out.println(yy + " status is: " + jobStatus);
                   Thread.sleep(1000);
               }
               yy++;
           }

           System.out.println("Job has Succeeded");
           GetJobOutputRequest jobOutputRequest = GetJobOutputRequest.builder()
               .jobId(jobId)
               .vaultName(name)
               .accountId(account)
               .build();

           ResponseBytes<GetJobOutputResponse> objectBytes = glacier.getJobOutputAsBytes(jobOutputRequest);
           // Write the data to a local file.
           byte[] data = objectBytes.asByteArray();
           File myFile = new File(path);
           OutputStream os = new FileOutputStream(myFile);
           os.write(data);
           System.out.println("Successfully obtained bytes from a Glacier vault");
           os.close();

       } catch(GlacierException | InterruptedException | IOException e) {
           System.out.println(e.getMessage());
           System.exit(1);

       }
    }
```
+  For API details, see [InitiateJob](https://docs.aws.amazon.com/goto/SdkForJavaV2/glacier-2012-06-01/InitiateJob) in *AWS SDK for Java 2\.x API Reference*\. 

------
#### [ Python ]

**SDK for Python \(Boto3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/python/example_code/glacier#code-examples)\. 
  

```
class GlacierWrapper:
    """Encapsulates Amazon S3 Glacier API operations."""
    def __init__(self, glacier_resource):
        """
        :param glacier_resource: A Boto3 Amazon S3 Glacier resource.
        """
        self.glacier_resource = glacier_resource


    @staticmethod
    def initiate_inventory_retrieval(vault):
        """
        Initiates an inventory retrieval job. The inventory describes the contents
        of the vault. Standard retrievals typically complete within 3—5 hours.
        When the job completes, you can get the inventory by calling get_output().

        :param vault: The vault to inventory.
        :return: The inventory retrieval job.
        """
        try:
            job = vault.initiate_inventory_retrieval()
            logger.info("Started %s job with ID %s.", job.action, job.id)
        except ClientError:
            logger.exception("Couldn't start job on vault %s.", vault.name)
            raise
        else:
            return job
```
+  For API details, see [InitiateJob](https://docs.aws.amazon.com/goto/boto3/glacier-2012-06-01/InitiateJob) in *AWS SDK for Python \(Boto3\) API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.