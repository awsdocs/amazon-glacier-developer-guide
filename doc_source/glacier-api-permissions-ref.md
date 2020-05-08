# Amazon S3 Glacier API Permissions: Actions, Resources, and Conditions Reference<a name="glacier-api-permissions-ref"></a>

When you are setting up [Access Control](auth-and-access-control.md#access-control) and writing a permissions policy that you can attach to an IAM identity \(identity\-based policies\) or a resource \(resource\-based policies\), you can use the following table as a reference\. The table lists each S3 Glacier API operation, the corresponding actions for which you can grant permissions to perform the action, and the AWS resource for which you can grant the permissions\.

You specify the actions in the policy's `Action` element, and you specify the resource value in the policy's `Resource` element\. Also, you can use the IAM policy language `Condition` element to specify when a policy should take effect\.

To specify an action, use the `glacier:` prefix followed by the API operation name \(for example, `glacier:CreateVault`\)\. For most S3 Glacier actions, `Resource` is the vault on which you want to grant the permissions\. You specify a vault as the `Resource` value by using the vault ARN\. To express conditions, you use predefined condition keys\. For more information, see [Overview of Managing Access Permissions to Your Amazon S3 Glacier Resources](access-control-overview.md)\.

The following table lists actions that can be used with identity\-based policies and resource\-based policies\. 

**Note**  
Some actions can only be used with identity\-based policies\. These actions are marked by a red asterisk \(*\**\) after the name of the API operation in the first column\.

Use the scroll bars to see the rest of the table\.


**Amazon S3 Glacier API and Required Permissions for Actions**  

| S3 Glacier API Operations | Required Permissions \(API Actions\) | Resources | Condition Keys | 
| --- | --- | --- | --- | 
| [Abort Multipart Upload \(DELETE uploadID\)](api-multipart-abort-upload.md)  | glacier:AbortMultipartUpload |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)  | glacier:AbortVaultLock |   |  | 
| [Add Tags To Vault \(POST tags add\)](api-AddTagsToVault.md) | glacier:AddTagsToVault |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md) | glacier:CompleteMultipartUpload |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  | `glacier:ResourceTag/TagKey` | 
| [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)  | glacier:CompleteVaultLock |   |  `glacier:ResourceTag/TagKey`  | 
| [Create Vault \(PUT vault\)](api-vault-put.md) \*  | glacier:CreateVault |   |  | 
| [Delete Archive \(DELETE archive\)](api-archive-delete.md) | glacier:DeleteArchive |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ArchiveAgeInDays` `glacier:ResourceTag/TagKey`  | 
| [Delete Vault \(DELETE vault\)](api-vault-delete.md) | glacier:DeleteVault |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Delete Vault Access Policy \(DELETE access\-policy\)](api-DeleteVaultAccessPolicy.md) | glacier:DeleteVaultAccessPolicy |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Delete Vault Notifications \(DELETE notification\-configuration\)](api-vault-notifications-delete.md) | glacier:DeleteVaultNotifications |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Describe Job \(GET JobID\)](api-describe-job-get.md) | glacier:DescribeJob |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  | 
| [Describe Vault \(GET vault\)](api-vault-get.md) | glacier:DescribeVault |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Get Data Retrieval Policy \(GET policy\)](api-GetDataRetrievalPolicy.md) \*  | glacier:GetDataRetrievalPolicy |  `arn:aws:glacier:region:account-id:policies/retrieval-limit-policy`  |  | 
| [Get Job Output \(GET output\)](api-job-output-get.md) | glacier:GetJobOutput |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Get Vault Access Policy \(GET access\-policy\)](api-GetVaultAccessPolicy.md) | glacier:GetVaultAccessPolicy |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)  | glacier:GetVaultLock |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Get Vault Notifications \(GET notification\-configuration\)](api-vault-notifications-get.md) | glacier:GetVaultNotifications |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [Initiate Job \(POST jobs\)](api-initiate-job-post.md) | glacier:InitiateJob |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ArchiveAgeInDays` `glacier:ResourceTag/TagKey`  | 
| [Initiate Multipart Upload \(POST multipart\-uploads\)](api-multipart-initiate-upload.md) | glacier:InitiateMultipartUpload |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)  | glacier:InitiateVaultLock |   |  `glacier:ResourceTag/TagKey`  | 
| [List Jobs \(GET jobs\)](api-jobs-get.md) | glacier:ListJobs |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [List Multipart Uploads \(GET multipart\-uploads\)](api-multipart-list-uploads.md) | glacier:ListMultipartUploads |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [List Parts \(GET uploadID\)](api-multipart-list-parts.md) | glacier:ListParts |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [List Tags For Vault \(GET tags\)](api-ListTagsForVault.md) | glacier:ListTagsForVault |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  | 
| [List Vaults \(GET vaults\)](api-vaults-get.md) | glacier:ListVaults |  |  | 
| [Remove Tags From Vault \(POST tags remove\)](api-RemoveTagsFromVault.md) | glacier:RemoveTagsFromVault |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Set Data Retrieval Policy \(PUT policy\)](api-SetDataRetrievalPolicy.md) \* | glacier:SetDataRetrievalPolicy | `arn:aws:glacier:region:account-id:policies/retrieval-limit-policy` |  | 
| [Set Vault Access Policy \(PUT access\-policy\)](api-SetVaultAccessPolicy.md) | glacier:SetVaultAccessPolicy |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md) | glacier:SetVaultNotifications |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Upload Archive \(POST archive\)](api-archive-post.md) | glacier:UploadArchive |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 
| [Upload Part \(PUT uploadID\)](api-upload-part.md) | glacier:UploadMultipartPart |  `arn:aws:glacier:region:account-id:vaults/vault-name` `arn:aws:glacier:region:account-id:vaults/example*` `arn:aws:glacier:region:account-id:vaults/*`  |  `glacier:ResourceTag/TagKey`  | 