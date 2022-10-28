# List tags for an Amazon S3 Glacier vault using an AWS SDK<a name="example_glacier_ListTagsForVault_section"></a>

The following code example shows how to list tags for an Amazon S3 Glacier vault\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Glacier#code-examples)\. 
  

```
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using Amazon.Glacier;
    using Amazon.Glacier.Model;

    class ListTagsForVault
    {
        static async Task Main(string[] args)
        {
            var client = new AmazonGlacierClient();
            var vaultName = "example-vault";

            var request = new ListTagsForVaultRequest
            {
                // Using a hyphen "=" for the Account Id will
                // cause the SDK to use the Account Id associated
                // with the default user.
                AccountId = "-",
                VaultName = vaultName,
            };

            var response = await client.ListTagsForVaultAsync(request);

            if (response.Tags.Count > 0)
            {
                foreach (KeyValuePair<string, string> tag in response.Tags)
                {
                    Console.WriteLine($"Key: {tag.Key}, value: {tag.Value}");
                }
            }
            else
            {
                Console.WriteLine($"{vaultName} has no tags.");
            }
        }
    }
```
+  For API details, see [ListTagsForVault](https://docs.aws.amazon.com/goto/DotNetSDKV3/glacier-2012-06-01/ListTagsForVault) in *AWS SDK for \.NET API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.