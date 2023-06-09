# Code examples for S3 Glacier using AWS SDKs<a name="service_code_examples"></a>

The following code examples show how to use S3 Glacier with an AWS software development kit \(SDK\)\. 

*Actions* are code excerpts from larger programs and must be run in context\. While actions show you how to call individual service functions, you can see actions in context in their related scenarios and cross\-service examples\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.

**Get started**

## Hello Amazon S3 Glacier<a name="example_glacier_Hello_section"></a>

The following code example shows how to get started using Amazon S3 Glacier\.

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/EventBridge#code-examples)\. 
  

```
using Amazon.Glacier;
using Amazon.Glacier.Model;

namespace GlacierActions;

public static class HelloGlacier
{
    static async Task Main()
    {
        var glacierService = new AmazonGlacierClient();

        Console.WriteLine("Hello Amazon Glacier!");
        Console.WriteLine("Let's list your Glacier vaults:");

        // You can use await and any of the async methods to get a response.
        // Let's get the vaults using a paginator.
        var glacierVaultPaginator = glacierService.Paginators.ListVaults(
            new ListVaultsRequest { AccountId = "-" });

        await foreach (var vault in glacierVaultPaginator.VaultList)
        {
            Console.WriteLine($"{vault.CreationDate}:{vault.VaultName}, ARN:{vault.VaultARN}");
        }
    }
}
```
+  For API details, see [ListVaults](https://docs.aws.amazon.com/goto/DotNetSDKV3/glacier-2012-06-01/ListVaults) in *AWS SDK for \.NET API Reference*\. 

------

**Contents**
+ [Actions](service_code_examples_actions.md)
  + [Add tags](example_glacier_AddTagsToVault_section.md)
  + [Create a multipart upload](example_glacier_UploadMultipartPart_section.md)
  + [Create a vault](example_glacier_CreateVault_section.md)
  + [Delete a vault](example_glacier_DeleteVault_section.md)
  + [Delete an archive](example_glacier_DeleteArchive_section.md)
  + [Delete vault notifications](example_glacier_DeleteVaultNotifications_section.md)
  + [Describe a job](example_glacier_DescribeJob_section.md)
  + [Describe a vault](example_glacier_DescribeVault_section.md)
  + [Download an archive](example_glacier_DownloadArchive_section.md)
  + [Get job output](example_glacier_GetJobOutput_section.md)
  + [Get vault notification configuration](example_glacier_GetVaultNotifications_section.md)
  + [List jobs](example_glacier_ListJobs_section.md)
  + [List tags](example_glacier_ListTagsForVault_section.md)
  + [List vaults](example_glacier_ListVaults_section.md)
  + [Retrieve a vault inventory](example_glacier_InitiateJob_InventoryRetrieval_section.md)
  + [Retrieve an archive from a vault](example_glacier_InitiateJob_ArchiveRetrieval_section.md)
  + [Set vault notifications](example_glacier_SetVaultNotifications_section.md)
  + [Upload an archive to a vault](example_glacier_UploadArchive_section.md)
+ [Scenarios](service_code_examples_scenarios.md)
  + [Archive a file, get notifications, and initiate a job](example_glacier_Usage_UploadNotifyInitiate_section.md)
  + [Get archive content and delete the archive](example_glacier_Usage_RetrieveDelete_section.md)