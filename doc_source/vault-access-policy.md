# Amazon S3 Glacier Access Control with Vault Access Policies<a name="vault-access-policy"></a>

An Amazon S3 Glacier vault access policy is a resource\-based policy that you can use to manage permissions to your vault\. For information about the different permissions policy options available, see [Managing Access to Resources](access-control-overview.md#access-control-manage-access-intro)\.

You can create one vault access policy for each vault to manage *permissions*\. You can modify permissions in a vault access policy at any time\. S3 Glacier also supports a Vault Lock policy on each vault that, after you lock it, cannot be altered\. For more information about working with Vault Lock policies, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\. 

You can use the Glacier API, AWS SDKs, AWS CLI, or the S3 Glacier console to create and manage vault access policies\. For a list of operations allowed for vault access resource\-based policies, see [Amazon S3 Glacier API Permissions: Actions, Resources, and Conditions Reference](glacier-api-permissions-ref.md)\.

**Topics**
+ [Example 1: Grant Cross\-Account Permissions for Specific Amazon S3 Glacier Actions](#vault-access-policy-example-multiple-accounts)
+ [Example 2: Grant Cross\-Account Permissions for MFA Delete Operations](#vault-access-policy-example-mfa-authentication)

## Example 1: Grant Cross\-Account Permissions for Specific Amazon S3 Glacier Actions<a name="vault-access-policy-example-multiple-accounts"></a>

The following example policy grants cross\-account permissions to two AWS accounts for a set of S3 Glacier operations on a vault named `examplevault`\.

**Note**  
The account that owns the vault is billed for all costs associated with the vault\. All requests, data transfer, and retrieval costs made by allowed external accounts are billed to the account that owns the vault\.

```
 1. {
 2.     "Version":"2012-10-17",
 3.     "Statement":[
 4.        {
 5.           "Sid":"cross-account-upload",
 6.           "Principal": {
 7.              "AWS": [
 8.                 "arn:aws:iam::123456789012:root",
 9.                 "arn:aws:iam::444455556666:root"
10.              ]
11.           },
12.           "Effect":"Allow",
13.           "Action": [
14.              "glacier:UploadArchive",
15.              "glacier:InitiateMultipartUpload",
16.              "glacier:AbortMultipartUpload",
17.              "glacier:CompleteMultipartUpload"
18.           ],
19.           "Resource": [
20.              "arn:aws:glacier:us-west-2:999999999999:vaults/examplevault"                                           
21.           ]
22.        }
23.     ]
24. }
```

## Example 2: Grant Cross\-Account Permissions for MFA Delete Operations<a name="vault-access-policy-example-mfa-authentication"></a>

You can use multi\-factor authentication \(MFA\) to protect your S3 Glacier resources\. To provide an extra level of security, MFA requires users to prove physical possession of an MFA device by providing a valid MFA code\. For more information about configuring MFA access, see [Configuring MFA\-Protected API Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/MFAProtectedAPI.html) in the *IAM User Guide*\. 

The example policy grants an AWS account with temporary credentials permission to delete archives from a vault named examplevault, provided the request is authenticated with an MFA device\. The policy uses the `aws:MultiFactorAuthPresent` condition key to specify this additional requirement\. For more information, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

```
 1. {
 2.     "Version": "2012-10-17",
 3.     "Statement": [
 4.        {
 5.           "Sid": "add-mfa-delete-requirement",
 6.           "Principal": {
 7.              "AWS": [
 8.                 "arn:aws:iam::123456789012:root"
 9.              ]
10.           },
11.           "Effect": "Allow",
12.           "Action": [ 
13.              "glacier:Delete*" 
14.           ],
15.           "Resource": [
16.              "arn:aws:glacier:us-west-2:999999999999:vaults/examplevault"
17.           ],
18.           "Condition": {
19.              "Bool": {
20.                 "aws:MultiFactorAuthPresent": true
21.              }
22.           }
23.        }
24.     ]
25. }
```

### Related Sections<a name="related-sections-vault-access-policy"></a>
+ [Delete Vault Access Policy \(DELETE access\-policy\)](api-DeleteVaultAccessPolicy.md)
+ [Get Vault Access Policy \(GET access\-policy\)](api-GetVaultAccessPolicy.md)
+ [Set Vault Access Policy \(PUT access\-policy\)](api-SetVaultAccessPolicy.md)