# Overview of Managing Access Permissions to Your Amazon S3 Glacier Resources<a name="access-control-overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and some services \(such as Amazon S3 Glacier \(Glacier\)\) also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\. 

When granting permissions, you decide who is getting the permissions, the resources they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [Amazon S3 Glacier Resources and Operations](#access-control-resources)
+ [Understanding Resource Ownership](#access-control-resource-ownership)
+ [Managing Access to Resources](#access-control-manage-access-intro)
+ [Specifying Policy Elements: Actions, Effects, Resources, and Principals](#access-control-specify-glacier-actions)
+ [Specifying Conditions in a Policy](#specifying-conditions)

## Amazon S3 Glacier Resources and Operations<a name="access-control-resources"></a>

In Glacier, the primary resource is a *vault*\. Glacier supports policies only at the vault level\. That is, in an IAM policy, the `Resource` value that you specify can be a specific vault or a set of vaults in a specific AWS Region\. Glacier doesn't support archive\-level permissions\. 

For all Glacier actions, `Resource` specifies the vault on which you want to grant the permissions\. These resources have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following table, and you can use a wildcard character \(\*\) in the ARN to match any vault name\.


****  

| Resource Type | ARN Format | 
| --- | --- | 
| Vaults | arn:aws:glacier:region:account\-id:vaults/vault\-name | 
| Vaults with names starting with "example" | arn:aws:glacier:region:account\-id:vaults/example\* | 
| All vaults in a region | arn:aws:glacier:region:account\-id:vaults/\* | 

Glacier provides a set of operations to work with the Glacier resources\. For information on the available operations, see [API Reference for Glacier](amazon-glacier-api.md)\.

## Understanding Resource Ownership<a name="access-control-resource-ownership"></a>

A *resource owner* is the AWS account that created the resource\. That is, the resource owner is the AWS account of the *principal entity* \(the root account, an IAM user, or an IAM role\) that authenticates the request that creates the resource\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to create a Glacier vault, your AWS account is the owner of the resource \(in Glacier, the resource is the Glacier vault\)\.
+ If you create an IAM user in your AWS account and grant permissions to create a Glacier vault to that user, the user can create a Glacier vault\. However, your AWS account, to which the user belongs, owns the Glacier vault resource\.
+ If you create an IAM role in your AWS account with permissions to create a Glacier vault, anyone who can assume the role can create a Glacier vault\. Your AWS account, to which the role belongs, owns the Glacier vault resource\. 

## Managing Access to Resources<a name="access-control-manage-access-intro"></a>

 A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\. 

**Note**  
This section discusses using IAM in the context of Glacier\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as *identity\-based* policies \(IAM polices\) and policies attached to a resource are referred to as *resource\-based* policies\. Glacier supports both identity\-based \(IAM policies\) and resource\-based policies\.

**Topics**
+ [Identity\-Based Policies \(IAM policies\)](#access-control-manage-access-intro-iam-policies)
+ [Resource\-Based Policies \(Amazon S3 Glacier Vault Policies\)](#access-control-manage-access-intro-resource-policies)

### Identity\-Based Policies \(IAM policies\)<a name="access-control-manage-access-intro-iam-policies"></a>

 You can attach policies to IAM identities\. For example you can do the following:
+ **Attach a permissions policy to a user or a group in your account –** An account administrator can use a permissions policy that is associated with a particular user to grant permissions for that user to create a Glacier vault\. 
+ **Attach a permissions policy to a role \(grant cross\-account permissions\) –** You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\.

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

  For more information about using IAM to delegate permissions, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. 

The following is an example policy that grants permissions for three Glacier vault\-related actions \(`glacier:CreateVault`, `glacier:DescribeVault` and `glacier:ListVaults`\) on a resource, using the Amazon Resource Name \(ARN\) that identifies all of the vaults in the `us-west-2` AWS Region\. ARNs uniquely identify AWS resources\. For more information about ARNs used with Glacier, see [Amazon S3 Glacier Resources and Operations](#access-control-resources)\.

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

The policy grants permissions to create, list, and obtain descriptions of vaults in the `us-west-2` region\. The wildcard character \(\*\) at the end of the ARN means that this statement can match any vault name\.

**Important**  
When you grant permissions to create a vault using the `glacier:CreateVault` operation, you must specify a wildcard character \(\*\) because you don't know the vault name until after you create the vault\.

For more information about using identity\-based policies with Glacier, see [Using Identity\-Based Policies for Amazon S3 Glacier \(IAM Policies\)](access-control-identity-based.md)\. For more information about users, groups, roles, and permissions, see [Identities \(Users, Groups, and Roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

 

### Resource\-Based Policies \(Amazon S3 Glacier Vault Policies\)<a name="access-control-manage-access-intro-resource-policies"></a>

Each Glacier vault can have resource\-based permissions policies associated with it\. For Glacier, a Glacier vault is the primary resource and resource\-based policies are referred to as *vault policies*\. 

You use Glacier vault policies to manage permissions in the following ways:
+ Manage user permissions in your account in a single vault policy, instead of individual user policies\.
+ Manage cross\-account permissions as an alternative to using IAM roles\.

A Glacier vault can have one vault access policy and one Vault Lock policy associated with it\. A Glacier *vault access policy* is a resource\-based policy that you can use to manage permissions to your vault\. A *Vault Lock policy* is a vault access policy that can be locked\. After you lock a Vault Lock policy, the policy cannot be changed\. You can use a Vault Lock policy to enforce compliance controls\.

You can use vault policies to grant permissions to all users, or you can limit access to a vault to a few AWS accounts by attaching a policy directly to a vault resource\. For example, you can use a Glacier vault policy to grant read\-only permissions to all AWS accounts or to grant permissions to upload archives to a few AWS accounts\. 

Vault policies make it easy to grant cross\-account access when you need to share your vault with other AWS accounts\. For example, you can grant read\-only access on a vault to a business partner with a different AWS account by simply including that account and allowed actions in the vault policy\. You can grant cross\-account access to multiple users in this fashion and have a single location to view all users with cross\-account access in the vault access policy\. For an example of a vault policy for cross\-account access, see [Example 1: Grant Cross\-Account Permissions for Specific Amazon S3 Glacier Actions](vault-access-policy.md#vault-access-policy-example-multiple-accounts)\. 

The following is an example of a Glacier vault policy \(a resource\-based policy\)\. The example policy grants all AWS accounts permissions to perform the `glacier:InitiateJob` and `glacier:GetJobOutput` actions\. This policy allows any AWS account to retrieve data from the specified vault\.

```
 1. {
 2.     "Version":"2012-10-17",
 3.     "Statement":[
 4.        {
 5.           "Sid": "add-read-only-perm",
 6.           "Principal": "*",
 7.           "Effect": "Allow",
 8.           "Action": [
 9.              "glacier:InitiateJob",
10.              "glacier:GetJobOutput"
11.           ],
12.           "Resource": [
13.              "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault"
14.           ]
15.        }
16.     ]
17.     }
```

For more information about using vault policies with Glacier, see [Using Resource\-Based Policies for Amazon S3 Glacier \(Vault Policies\)](access-control-resource-based.md)\. For additional information about IAM roles \(identity\-based policies\) as opposed to resource\-based policies, see [How IAM Roles Differ from Resource\-based Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_compare-resource-policies.html) in the *IAM User Guide*\.

## Specifying Policy Elements: Actions, Effects, Resources, and Principals<a name="access-control-specify-glacier-actions"></a>

For each type of Glacier resource, the service defines a set of API operations \(see [API Reference for Glacier](amazon-glacier-api.md)\)\. To grant permissions for these API operations Glacier defines a set of actions that you can specify in a policy\. Note that, performing an API operation can require permissions for more than one action\. When granting permissions for specific actions, you also identify the resource on which the actions are allowed or denied\.

The following are the most basic policy elements:
+ **Resource** – In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource to which the policy applies\. For more information, see [Amazon S3 Glacier Resources and Operations](#access-control-resources)\. 
+ **Actions** – You use action keywords to identify resource operations that you want to allow or deny\. 

  For example, the `glacier:CreateVault` permission allows the user permissions to perform the Glacier `Create Vault` operation\. 
+ **Effect** – You specify the effect when the user requests the specific action—this can be either allow or deny\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. 

To learn more about the IAM policy syntax, and descriptions, see [AWS IAM Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the Glacier API actions and the resources that they apply to, see [Amazon S3 Glacier API Permissions: Actions, Resources, and Conditions Reference](glacier-api-permissions-ref.md)\. 

## Specifying Conditions in a Policy<a name="specifying-conditions"></a>

When you grant permissions, you can use the IAM policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\.

AWS provides a set of predefined condition keys, called *AWS\-wide condition keys*, for all AWS services that support IAM for access control\. AWS\-wide condition keys use the prefix `aws`\. Glacier supports all AWS\-wide condition keys in vault access and Vault Lock policies\. For example, you can use the `aws:MultiFactorAuthPresent` condition key to require multi\-factor authentication \(MFA\) when requesting an action\. For more information and a list of the AWS\-wide condition keys, see [Available Keys for Conditions](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\.

**Note**  
Condition keys are case\-sensitive\.

In addition, Glacier also provides its own condition keys that you can include in `Condition` elements in an IAM permissions policy\. Glacier–specific condition keys are applicable only when granting Glacier–specific permissions\. Glacier condition key names have the prefix `glacier:`\. The following table shows the Glacier condition keys that apply to Glacier resources\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/access-control-overview.html)

For examples of using the Glacier–specific condition keys, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.

Related Topics
+ [Using Identity\-Based Policies for Amazon S3 Glacier \(IAM Policies\)](access-control-identity-based.md)
+ [Using Resource\-Based Policies for Amazon S3 Glacier \(Vault Policies\)](access-control-resource-based.md)
+ [Amazon S3 Glacier API Permissions: Actions, Resources, and Conditions Reference](glacier-api-permissions-ref.md)