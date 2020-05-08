# Using Identity\-Based Policies for Amazon S3 Glacier \(IAM Policies\)<a name="access-control-identity-based"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
 We recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your Amazon S3 Glacier \(S3 Glacier\) resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon S3 Glacier Resources](access-control-overview.md)\.

The sections in this topic cover the following:
+ [Permissions Required to Use the Amazon S3 Glacier Console](#additional-console-required-permissions) 
+ [AWS Managed Policies \(Predefined Policies\) for Amazon S3 Glacier](#access-policy-examples-aws-managed) 
+ [Customer Managed Policy Examples](#access-policy-examples-for-sdk-cli) 

The following shows an example of a permissions policy\.

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

The policy grants permissions for three S3 Glacier vault\-related actions \(`glacier:CreateVault`, `glacier:DescribeVault` and `glacier:ListVaults`\), on a resource using the Amazon Resource Name \(ARN\) that identifies all of the vaults in the `us-west-2` AWS Region\. 

The wildcard character \(\*\) at the end of the ARN means that this statement can match any vault name\. The statement allows the `glacier:DescribeVault` action on any vault in the specified AWS Region, `us-west-2`\. If you want to limit permissions for this action to a specific vault only, you replace the wildcard character \(\*\) with a vault name\. 

## Permissions Required to Use the Amazon S3 Glacier Console<a name="additional-console-required-permissions"></a>

The S3 Glacier console provides an integrated environment for you to create and manage S3 Glacier vaults\. At a minimum IAM users that you create must be granted permissions for the `glacier:ListVaults` action to view the S3 Glacier console as shown in the following example\. 

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

Both of the S3 Glacier AWS Managed policies discussed in the next section grant permissions for `glacier:ListVaults`\. 

## AWS Managed Policies \(Predefined Policies\) for Amazon S3 Glacier<a name="access-policy-examples-aws-managed"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to S3 Glacier:
+ **AmazonGlacierReadOnlyAccess** – Grants read only access to S3 Glacier through the AWS Management Console\.
+ **AmazonGlacierFullAccess** – Grants full access to S3 Glacier through the AWS Management Console\. 

**Note**  
You can review these permissions policies by signing in to the IAM console and searching for specific policies there\.

You can also create your own custom IAM policies to allow permissions for S3 Glacier API actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions or to custom execution roles \(IAM roles\) that you create for your S3 Glacier vaults\. 

## Customer Managed Policy Examples<a name="access-policy-examples-for-sdk-cli"></a>

In this section, you can find example user policies that grant permissions for various S3 Glacier actions\. These policies work when you are using S3 Glacier REST API, the AWS SDKs, the AWS CLI, or, if applicable, the S3 Glacier management console\. 

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

**Topics**
+ [Example 1: Allow a User to Download Archives from a Vault](#vault-access-policy-example-init-jobs)
+ [Example 2: Allow a User to Create a Vault and Configure Notifications](#vault-access-policy-example-create-vault)
+ [Example 3: Allow a User to Upload Archives to a Specific Vault](#vault-access-policy-example-upload-archives)
+ [Example 4: Allow a User Full Permissions on a Specific Vault](#vault-access-policy-example-full-permission)

### Example 1: Allow a User to Download Archives from a Vault<a name="vault-access-policy-example-init-jobs"></a>

To download an archive, you first initiate a job to retrieve the archive\. After the retrieval job is complete, you can download the data\. The following example policy grants permissions for the `glacier:InitiateJob` action to initiate a job \(which allows the user to retrieve an archive or a vault inventory from the vault\), and permissions for the `glacier:GetJobOutput` action to download the retrieved data\. The policy also grants permissions to perform the `glacier:DescribeJob` action so that the user can get the job status\. For more information, see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\.

The policy grants these permissions on a vault named `examplevault`\. You can get the vault ARN from the [Amazon S3 Glacier console](https://console.aws.amazon.com/glacier), or programmatically by calling either the [Describe Vault \(GET vault\)](api-vault-get.md) or the [List Vaults \(GET vaults\)](api-vaults-get.md) API actions\.

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