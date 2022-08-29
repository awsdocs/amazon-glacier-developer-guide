# Add tags to an Amazon S3 Glacier vault using an AWS SDK<a name="example_glacier_AddTagsToVault_section"></a>

The following code example shows how to add tags to an Amazon S3 Glacier vault\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ \.NET ]

**AWS SDK for \.NET**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Glacier#code-examples)\. 
  

```
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using Amazon.Glacier;
    using Amazon.Glacier.Model;

    public class AddTagsToVault
    {
        public static async Task Main(string[] args)
        {
            string vaultName = "example-vault";

            var client = new AmazonGlacierClient();
            var request = new AddTagsToVaultRequest
            {
                Tags = new Dictionary<string, string>
                {
                    { "examplekey1", "examplevalue1" },
                    { "examplekey2", "examplevalue2" },
                },
                AccountId = "-",
                VaultName = vaultName,
            };

            var response = await client.AddTagsToVaultAsync(request);
        }
    }
```
+  For API details, see [AddTagsToVault](https://docs.aws.amazon.com/goto/DotNetSDKV3/glacier-2012-06-01/AddTagsToVault) in *AWS SDK for \.NET API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.