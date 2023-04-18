# Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS Command Line Interface<a name="retrieving-vault-info-cli"></a>

This example shows how to retrieve vault information and metadata in Amazon S3 Glacier \(S3 Glacier\) using the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Retrieving Vault Metadata Using the AWS CLI](#Retrieving-Vault-Metadata-CLI-Implementation)

## \(Prerequisite\) Setting Up the AWS CLI<a name="Creating-Vaults-CLI-Setup"></a>

1. Download and configure the AWS CLI\. For instructions, see the following topics in the *AWS Command Line Interface User Guide*: 

    [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) 

   [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)

1. Verify your AWS CLI setup by entering the following commands at the command prompt\. These commands don't provide credentials explicitly, so the credentials of the default profile are used\.
   + Try using the help command\.

     ```
     aws help
     ```
   + To get a list of S3 Glacier vaults on the configured account, use the `list-vaults` command\. Replace *123456789012* with your AWS account ID\.

     ```
     aws glacier list-vaults --account-id 123456789012
     ```
   + To see the current configuration data for the AWS CLI, use the `aws configure list` command\.

     ```
     aws configure list
     ```

## Example: Retrieving Vault Metadata Using the AWS CLI<a name="Retrieving-Vault-Metadata-CLI-Implementation"></a>
+ Use the `describe-vault` command to describe a vault named *awsexamplevault* under account *111122223333*\.

  ```
  aws glacier describe-vault --vault-name awsexamplevault --account-id 111122223333
  ```