# Step 2: Create a Vault in Amazon S3 Glacier<a name="getting-started-create-vault"></a>

A vault is a container for storing archives\. Your first step is to create a vault in one of the supported AWS Regions\. In this getting started exercise, you create a vault in the US West \(Oregon\) Region\. For a list of the AWS Regions supported by Amazon S3 Glacier \(S3 Glacier\), go to [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#glacier_region) in the *AWS General Reference*\.

You can create vaults programmatically or by using the S3 Glacier console\. This section uses the console to create a vault\. In a later step, you will upload an archive to the vault\.

**To create a vault**

1. Sign into the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/](https://console.aws.amazon.com/glacier/)\.

1. Select an AWS Region from the Region selector\.

   In this getting started exercise, we use the US West \(Oregon\) Region\.

1. If you are using S3 Glacier for the first time, click **Get started**\. \(Otherwise, you would click **Create Vault**\.\)  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-first-run.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. Enter **examplevault** as the vault name in the **Vault Name** field and then click **Next Step**\.

   There are guidelines for naming a vault\. For more information, see [Creating a Vault in Amazon S3 Glacier](creating-vaults.md)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. Select **Do not enable notifications**\. For this getting started exercise, you will not configure notifications for the vault\.

   If you wanted to have notifications sent to you or your application whenever certain S3 Glacier jobs complete, you would select **Enable notifications and create a new SNS topic** or **Enable notifications and use an existing SNS topic** to set up Amazon Simple Notification Service \(Amazon SNS\) notifications\. In subsequent steps, you upload an archive and then download it using the high\-level API of the AWS SDK\. Using the high\-level API does not require that you configure vault notification to retrieve your data\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-set-notifications.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. If the AWS Region and vault name are correct, then click **Submit**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-review.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

1. Your new vault is listed on the **S3 Glacier Vaults** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/glacier-create-vault-list.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)