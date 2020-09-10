# Amazon S3 Glacier Vault Lock<a name="vault-lock"></a>

The following topics describe how to lock a vault in Amazon S3 Glacier and how to use Vault Lock policies\.

**Topics**
+ [Vault Locking Overview](#vault-lock-overview)
+ [Locking a Vault by Using the Amazon S3 Glacier API](vault-lock-how-to-api.md)

## Vault Locking Overview<a name="vault-lock-overview"></a>

S3 Glacier Vault Lock allows you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a vault lock policy\. You can specify controls such as “write once read many” \(WORM\) in a vault lock policy and lock the policy from future edits\. Once locked, the policy can no longer be changed\. 

S3 Glacier enforces the controls set in the vault lock policy to help achieve your compliance objectives, for example, for data retention\. You can deploy a variety of compliance controls in a vault lock policy using the AWS Identity and Access Management \(IAM\) policy language\. For more information about vault lock policies, see [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)\.

A vault lock policy is different than a vault access policy\. Both policies govern access controls to your vault\. However, a vault lock policy can be locked to prevent future changes, providing strong enforcement for your compliance controls\. You can use the vault lock policy to deploy regulatory and compliance controls, which typically require tight controls on data access\. In contrast, you use a vault access policy to implement access controls that are not compliance related, temporary, and subject to frequent modification\. Vault lock and vault access policies can be used together\. For example, you can implement time\-based data retention rules in the vault lock policy \(deny deletes\), and grant read access to designated third parties or your business partners \(allow reads\)\.

Locking a vault takes two steps: 

1. Initiate the lock by attaching a vault lock policy to your vault, which sets the lock to an in\-progress state and returns a lock ID\. While in the in\-progress state, you have 24 hours to validate your vault lock policy before the lock ID expires\.

1. Use the lock ID to complete the lock process\. If the vault lock policy doesn't work as expected, you can stop the lock and restart from the beginning\. For information on how to use the S3 Glacier API to lock a vault, see [Locking a Vault by Using the Amazon S3 Glacier API](vault-lock-how-to-api.md)\.