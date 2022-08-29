# Locking a Vault using the AWS Command Line Interface<a name="vault-lock-how-to-cli"></a>

You can lock your vault using the AWS Command Line Interface\. This will install a vault lock policy on the specified vault and return the lock ID\. You must complete the vault locking process within 24 hours else the vault lock policy is removed from the vault\.

## \(Prerequisite\) Setting Up the AWS CLI<a name="Creating-Vaults-CLI-Setup"></a>

1. Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*\. 

    [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 

   [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

1. Verify the setup by entering the following commands at the command prompt\. These commands don't provide credentials explicitly, so the credentials of the default profile are used\.
   + Try using the help command\.

     ```
     aws help
     ```
   + Use `aws s3 ls` to get a list of buckets on the configured account\.

     ```
     aws s3 ls
     ```
   + Use `aws configure list` to see the current configuration data\.

     ```
     aws configure list
     ```

1. Use the `initiate-vault-lock` to install a vault lock policy and sets the lock state of the vault lock to `InProgress`\.

   ```
   aws glacier initiate-vault-lock --vault-name examplevault --account-id 111122223333 --policy file://lockconfig.json
   ```

1.  The lock configuration is a JSON document as shown in the following example\. 

   ```
   ```

   For more information about using Amazon SNS topics for Glacier see, [Configuring Vault Notifications in S3 Glacier: General Concepts](configuring-notifications.html#configuring-notifications.general)

   For more information about Amazon SNS, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/gsg/Welcome.html)\.

## Related Sections<a name="related-sections-vault-lock-how-to-cli"></a>
+ [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)