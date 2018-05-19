# Creating a Vault in Amazon Glacier<a name="creating-vaults"></a>

Creating a vault adds a vault to the set of vaults in your account\. An AWS account can create up to 1,000 vaults per region\. For a list of the AWS regions supported by Amazon Glacier, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#glacier_region) in the *AWS General Reference*\. For information on creating more vaults, go to the [Amazon Glacier product detail page](http://aws.amazon.com/glacier)\. 

When you create a vault, you must provide a vault name\. The following are the vault naming requirements: 
+  Names can be between 1 and 255 characters long\. 
+ Allowed characters are a–z, A–Z, 0–9, '\_' \(underscore\), '\-' \(hyphen\), and '\.' \(period\)\.

Vault names must be unique within an account and the region in which the vault is being created\. That is, an account can create vaults with the same name in different regions but not in the same region\.  

**Topics**
+ [Creating a Vault in Amazon Glacier Using the AWS SDK for Java](creating-vaults-sdk-java.md)
+ [Creating a Vault in Amazon Glacier Using the AWS SDK for \.NET](creating-vaults-dotnet-sdk.md)
+ [Creating a Vault in Amazon Glacier Using the REST API](creating-vaults-rest-api.md)
+ [Creating a Vault Using the Amazon Glacier Console](creating-vaults-console.md)