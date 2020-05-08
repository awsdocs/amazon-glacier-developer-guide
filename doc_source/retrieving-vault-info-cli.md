# Retrieving Vault Metadata in Amazon S3 Glacier Using the AWS Command Line Interface<a name="retrieving-vault-info-cli"></a>

This example shows how to retrieve vault information and metadata in Amazon S3 Glacier \(S3 Glacier\) using the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Retrieving Vault Metadata Using the AWS CLI](#Retrieving-Vault-Metadata-CLI-Implementation)

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

## Example: Retrieving Vault Metadata Using the AWS CLI<a name="Retrieving-Vault-Metadata-CLI-Implementation"></a>
+ Use the `describe-vault` command to describe a vault named *awsexamplevault* under account *111122223333*\.

  ```
  aws glacier describe-vault --vault-name awsexamplevault --account-id 111122223333
  ```