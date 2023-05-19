# Locking a Vault by Using the S3 Glacier Console<a name="vault-lock-walkthrough"></a>

Amazon S3 Glacier Vault Lock helps you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a Vault Lock policy\. For more information about S3 Glacier Vault Lock, see [Amazon S3 Glacier Access Control with Vault Lock Policies](https://docs.aws.amazon.com/amazonglacier/latest/dev/vault-lock-policy.html)\. 

**Important**  
We recommend that you first create a vault, complete a Vault Lock policy, and then upload your archives to the vault so that the policy will be applied to them\.
After the Vault Lock policy is locked, it cannot be changed or deleted\.

**To initiate a Vault Lock policy on your vault by using the S3 Glacier console**

You initiate the lock by attaching a Vault Lock policy to your vault, which sets the lock to an in\-progress state and returns a lock ID\. While the policy is in the in\-progress state, you have 24 hours to validate your Vault Lock policy before the lock ID expires\. 

1. Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/home](https://console.aws.amazon.com/glacier/home)\.

1. Under **Select a Region**, select an AWS Region from the Region selector\.

1. In the left navigation pane, choose **Vaults**\.

1. On the **Vaults** page, choose **Create vault**\.

1. Create a new vault\.
**Important**  
We recommend that you first create a vault, complete a Vault Lock policy, and then upload your archives to the vault so that the policy will be applied to them\. 

1.  Choose your new vault from the **Vaults** list\.

1.  Choose the **Vault policies** tab\.

1. In the **Vault Lock policy** section, choose **Initiate Vault Lock policy**\. 

1. On the **Initiate Vault Lock policy** page, specify the record retention controls in your Vault Lock policy in text format in the standard text box\.
**Note**  
You can specify the record retention controls in a Vault Lock policy in text format and initiate the Vault Lock by calling the `Initiate Vault Lock` API operation or through the interactive UI in the S3 Glacier console\. For information about formatting your Vault Lock policy, see [Amazon S3 Glacier Vault Lock Policy Examples](https://docs.aws.amazon.com/amazonglacier/latest/dev/vault-lock-policy.html#vault-lock-policy-example-deny-delete-archive-age)\. 

1. Choose **Save changes**\.

1. In the **Record Vault Lock ID** dialog box, copy your **Lock ID** and save it in a safe place\. 
**Important**  
After the Vault Lock policy has been initiated, you have 24 hours to validate the policy and complete the lock process\. To complete the lock process, you must provide the lock ID\. If it's not provided within 24 hours, the lock ID expires and your in\-progress policy is deleted\.

1. After saving your lock ID in a safe place, choose **Close**\.

1. Test your Vault Lock policy within the next 24 hours\. If the policy is working as intended, choose **Complete Vault Lock policy**\.

1. In the **Complete Vault Lock** dialog box, select the check box to acknowledge that completing the Vault Lock policy process is irreversible\.

1. Enter your provided **Lock ID** in the text box\.

1. Choose **Complete Vault Lock**\.