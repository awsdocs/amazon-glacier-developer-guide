# Identity\-based policy examples for Amazon S3 Glacier<a name="security_iam_id-based-policy-examples"></a>

By default, users and roles don't have permission to create or modify S3 Glacier resources\. They also can't perform tasks by using the AWS Management Console, AWS Command Line Interface \(AWS CLI\), or AWS API\. To grant users permission to perform actions on the resources that they need, an IAM administrator can create IAM policies\. The administrator can then add the IAM policies to roles, and users can assume the roles\.

To learn how to create an IAM identity\-based policy by using these example JSON policy documents, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) in the *IAM User Guide*\.

For details about actions and resource types defined by S3 Glacier, including the format of the ARNs for each of the resource types, see [Actions, resources, and condition keys for Amazon S3 Glacier](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazons3glacier.html) in the *Service Authorization Reference*\.

The following is an example policy that grants permissions for three S3 Glacier vault\-related actions \(`glacier:CreateVault`, `glacier:DescribeVault` and `glacier:ListVaults`\) on a resource, using the Amazon Resource Name \(ARN\) that identifies all of the vaults in the `us-west-2` AWS Region\. ARNs uniquely identify AWS resources\. For more information about ARNs used with S3 Glacier, see [Policy resources for S3 Glacier](security_iam_service-with-iam.md#security_iam_service-with-iam-id-based-policies-resources)\.

```
      {
         "Version": "2012-10-17",
         "Statement": [
            {
               "Effect": "Allow",
               "Action": [
               "glacier:CreateVault",
               "glacier:DescribeVault",
               "glacier:ListVaults"
               ],
               "Resource": "arn:aws:glacier:us-west-2:123456789012:vaults/*"
            }
         ]
      }
```

The policy grants permissions to create, list, and obtain descriptions of vaults in the `us-west-2` Region\. The wildcard character \(\*\) at the end of the ARN means that this statement can match any vault name\.

**Important**  
When you grant permissions to create a vault using the `glacier:CreateVault` operation, you must specify a wildcard character \(\*\) because you don't know the vault name until after you create the vault\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the S3 Glacier console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#id-based-policy-view-own-permissions)
+ [Customer Managed Policy Examples](#id-based-policy-customer-managed)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete S3 Glacier resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or a root user in your AWS account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the S3 Glacier console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Amazon S3 Glacier console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the S3 Glacier resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(users or roles\) with that policy\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that they're trying to perform\.

The S3 Glacier console provides an integrated environment for you to create and manage S3 Glacier vaults\. At a minimum IAM identities that you create must be granted permissions for the `glacier:ListVaults` action to view the S3 Glacier console as shown in the following example\. 

```
         {
               "Version": "2012-10-17",
               "Statement": [
                  {
                     "Action": [
                     "glacier:ListVaults"     
                     ],
                     "Effect": "Allow",
                     "Resource": "*"
                  }
               ]
            }
```

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to S3 Glacier:

 
+ **AmazonGlacierReadOnlyAccess** – Grants read only access to S3 Glacier through the AWS Management Console\.
+ **AmazonGlacierFullAccess** – Grants full access to S3 Glacier through the AWS Management Console\. 

 

You can also create your own custom IAM policies to allow permissions for S3 Glacier API actions and resources\. You can attach these custom policies to the custom IAM roles that you create for your S3 Glacier vaults\. 

Both of the S3 Glacier AWS Managed policies discussed in the next section grant permissions for `glacier:ListVaults`\. 

For more information, see [Adding permissions to a user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

## Allow users to view their own permissions<a name="id-based-policy-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Customer Managed Policy Examples<a name="id-based-policy-customer-managed"></a>

In this section, you can find example user policies that grant permissions for various S3 Glacier actions\. These policies work when you are using S3 Glacier REST API, the Amazon SDKs, the AWS CLI, or, if applicable, the S3 Glacier management console\. 

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

**Topics**
+ [Example 1: Allow a User to Download Archives from a Vault](#vault-access-policy-example-init-jobs)
+ [Example 2: Allow a User to Create a Vault and Configure Notifications](#vault-access-policy-example-create-vault)
+ [Example 3: Allow a User to Upload Archives to a Specific Vault](#vault-access-policy-example-upload-archives)
+ [Example 4: Allow a User Full Permissions on a Specific Vault](#vault-access-policy-example-full-permission)

### Example 1: Allow a User to Download Archives from a Vault<a name="vault-access-policy-example-init-jobs"></a>

To download an archive, you first initiate a job to retrieve the archive\. After the retrieval job is complete, you can download the data\. The following example policy grants permissions for the `glacier:InitiateJob` action to initiate a job \(which allows the user to retrieve an archive or a vault inventory from the vault\), and permissions for the `glacier:GetJobOutput` action to download the retrieved data\. The policy also grants permissions to perform the `glacier:DescribeJob` action so that the user can get the job status\. For more information, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.

The policy grants these permissions on a vault named `examplevault`\. You can get the vault ARN from the [Amazon S3 Glacier console](https://console.aws.amazon.com/glacier/home), or programmatically by calling either the [Describe Vault \(GET vault\)](api-vault-get.md) or the [List Vaults \(GET vaults\)](api-vaults-get.md) API actions\.

```
{
               "Version":"2012-10-17",
               "Statement": [
                  {
                     "Effect": "Allow",
                     "Resource": "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault",
                     "Action":["glacier:InitiateJob",
                              "glacier:GetJobOutput",
                              "glacier:DescribeJob"] 
                  }
               ]
               }
```

### Example 2: Allow a User to Create a Vault and Configure Notifications<a name="vault-access-policy-example-create-vault"></a>

The following example policy grants permissions to create a vault in the us\-west\-2 Region as specified in the `Resource` element and configure notifications\. For more information about working with notifications, see [Configuring Vault Notifications in Amazon S3 Glacier](configuring-notifications.md)\. The policy also grants permissions to list vaults in the AWS Region and get a specific vault description\. 

**Important**  
When you grant permissions to create a vault using the `glacier:CreateVault` operation, you must specify a wildcard character \(\*\) in the `Resource` value because you don't know the vault name until after you create the vault\.

```
{
               "Version":"2012-10-17",
               "Statement": [
                  {
                     "Effect": "Allow",
                     "Resource": "arn:aws:glacier:us-west-2:123456789012:vaults/*",
                     "Action":["glacier:CreateVault",
                              "glacier:SetVaultNotifications",
                              "glacier:GetVaultNotifications",
                              "glacier:DeleteVaultNotifications",
                              "glacier:DescribeVault",
                              "glacier:ListVaults"] 
                  }
               ]
               }
```

### Example 3: Allow a User to Upload Archives to a Specific Vault<a name="vault-access-policy-example-upload-archives"></a>

The following example policy grants permissions to upload archives to a specific vault in the us\-west\-2 Region\. These permissions allow a user to upload an archive all at once using the [Upload Archive \(POST archive\)](api-archive-post.md) API operation or in parts using the [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md) API operation\.



```
{
               "Version":"2012-10-17",
               "Statement": [
                  {
                     "Effect": "Allow",
                     "Resource": "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault",
                     "Action":["glacier:UploadArchive",
                              "glacier:InitiateMultipartUpload",
                              "glacier:UploadMultipartPart",
                              "glacier:ListParts",
                              "glacier:ListMultipartUploads",
                              "glacier:CompleteMultipartUpload"] 
                  }
               ]
               }
```

### Example 4: Allow a User Full Permissions on a Specific Vault<a name="vault-access-policy-example-full-permission"></a>

The following example policy grants permissions for all S3 Glacier actions on a vault named `examplevault`\.

```
{
            "Version":"2012-10-17",
            "Statement": [
               {
                  "Effect": "Allow",
                  "Resource": "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault",
                  "Action":["glacier:*"] 
               }
            ]
            }
```





