# Using Resource\-Based Policies for Amazon S3 Glacier \(Vault Policies\)<a name="access-control-resource-based"></a>

An Amazon S3 Glacier \(S3 Glacier\) vault is the primary resource in S3 Glacier\. You can add permissions to the policy associated with a S3 Glacier vault\. Permissions policies attached to S3 Glacier vaults are referred to as *resource\-based policies* \(or *vault policies* in S3 Glacier\)\. Each S3 Glacier vault can have resource\-based permissions policies associated with it\.  For information about available permissions policy options, see [Managing Access to Resources](access-control-overview.md#access-control-manage-access-intro)\.

**Important**  
Before you create resource\-based policies, we recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your S3 Glacier resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon S3 Glacier Resources](access-control-overview.md)\.

A S3 Glacier vault can have one vault access policy and one Vault Lock policy associated with it\. An Amazon S3 Glacier *vault access policy* is a resource\-based policy that you can use to manage permissions to your vault\. A *Vault Lock policy* is vault access policy that can be locked\. After you lock a Vault Lock policy, the policy can't be changed\. You can use a Vault Lock Policy to enforce compliance controls\.

For more information, see the following topics\.

**Topics**
+ [Amazon S3 Glacier Access Control with Vault Access Policies](vault-access-policy.md)
+ [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)