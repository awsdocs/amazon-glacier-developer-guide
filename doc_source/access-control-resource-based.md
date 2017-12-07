# Using Resource\-Based Policies for Amazon Glacier \(Vault Policies\)<a name="access-control-resource-based"></a>

An Amazon Glacier vault is the primary resource in Amazon Glacier\. You can add permissions to the policy associated with a Amazon Glacier vault\. Permissions policies attached to Amazon Glacier vaults are referred to as *resource\-based policies* \(or *vault policies* in Amazon Glacier\)\. Each Amazon Glacier vault can have resource\-based permissions policies associated with it\.  For information about available permissions policy options, see [Managing Access to Resources](access-control-overview.md#access-control-manage-access-intro)\.

**Important**  
Before you create resource\-based policies, we recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your Amazon Glacier resources\. For more information, see [Overview of Managing Access Permissions to Your Amazon Glacier Resources](access-control-overview.md)\.

An Amazon Glacier vault can have one vault access policy and one Vault Lock policy associated with it\. An Amazon Glacier *vault access policy* is a resource\-based policy that you can use to manage permissions to your vault\. A *Vault Lock policy* is vault access policy that can be locked\. After you lock a Vault Lock policy, the policy can't be changed\. You can use a Vault Lock Policy to enforce compliance controls\.

For more information, see the following topics\.


+ [Amazon Glacier Access Control with Vault Access Policies](vault-access-policy.md)
+ [Amazon Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)