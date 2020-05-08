# Retrieving Vault Metadata in Amazon S3 Glacier<a name="retrieving-vault-info"></a>

You can retrieve vault information such as the vault creation date, number of archives in the vault, and the total size of all the archives in the vault\. Amazon S3 Glacier \(S3 Glacier\) provides API calls for you to retrieve this information for a specific vault or all the vaults in a specific AWS Region in your account\. 

If you retrieve a vault list, S3 Glacier returns the list sorted by the ASCII values of the vault names\. The list contains up to 1,000 vaults\. You should always check the response for a marker at which to continue the list; if there are no more items the marker field is `null`\. You can optionally limit the number of vaults returned in the response\. If there are more vaults than are returned in the response, the result is paginated\. You need to send additional requests to fetch the next set of vaults\. 

**Topics**
+ [Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS SDK for Java](retrieving-vault-info-sdk-java.md)
+ [Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS SDK for \.NET](retrieving-vault-info-sdk-dotnet.md)
+ [Retrieving Vault Metadata Using the REST API](listing-vaults-rest-api.md)
+ [Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS Command Line Interface](retrieving-vault-info-cli.md)