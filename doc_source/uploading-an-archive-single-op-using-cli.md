# Uploading an Archive in a Single Operation Using the AWS Command Line Interface<a name="uploading-an-archive-single-op-using-cli"></a>

You can upload an archive in Amazon S3 Glacier \(S3 Glacier\) using the AWS Command Line Interface \(AWS CLI\)\.

**Topics**
+ [\(Prerequisite\) Setting Up the AWS CLI](#Creating-Vaults-CLI-Setup)
+ [Example: Upload an Archive Using the AWS CLI](#Uploading-Archives-CLI-Implementation)

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

## Example: Upload an Archive Using the AWS CLI<a name="Uploading-Archives-CLI-Implementation"></a>

In order to upload an archive you must have a vault created\. For more information about creating vaults, see [Creating a Vault in Amazon S3 Glacier](creating-vaults.md)\.

1. Use the `upload-archive` command to add an archive to an existing vault\. In the below example replace the `vault name` and `account ID`\. For the `body` parameter specify a path to the file you wish to upload\.

   ```
   aws glacier upload-archive --vault-name awsexamplevault --account-id 123456789012 --body archive.zip
   ```

1.  Expected output:

   ```
   {
       "archiveId": "kKB7ymWJVpPSwhGP6ycSOAekp9ZYe_--zM_mw6k76ZFGEIWQX-ybtRDvc2VkPSDtfKmQrj0IRQLSGsNuDp-AJVlu2ccmDSyDUmZwKbwbpAdGATGDiB3hHO0bjbGehXTcApVud_wyDw",
       "checksum": "969fb39823836d81f0cc028195fcdbcbbe76cdde932d4646fa7de5f21e18aa67",
       "location": "/123456789012/vaults/awsexamplevault/archives/kKB7ymWJVpPSwhGP6ycSOAekp9ZYe_--zM_mw6k76ZFGEIWQX-ybtRDvc2VkPSDtfKmQrj0IRQLSGsNuDp-AJVlu2ccmDSyDUmZwKbwbpAdGATGDiB3hHO0bjbGehXTcApVud_wyDw"
   }
   ```

   When finished the command will output the archive ID, checksum, and location in S3 Glacier\. For more information about the upload\-archive command, see [upload\-archive](https://docs.aws.amazon.com/cli/latest/reference/glacier/upload-archive.html) in the *AWS CLI Command Reference*\.