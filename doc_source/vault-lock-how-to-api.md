# Locking a Vault by Using the Amazon S3 Glacier API<a name="vault-lock-how-to-api"></a>

To lock your vault with the Glacier API, you first call [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) with a vault lock policy that specifies the controls you want to deploy\. [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) attaches the policy to your vault, transitions the vault lock to the in\-progress state, and returns a unique lock ID\. After the vault lock enters the in\-progress state, you have 24 hours to complete the lock by calling [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md) with the lock ID returned from [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)\. After the vault is locked it cannot be unlocked\.

 If you don't complete the vault lock process within 24 hours after entering the in\-progress state, your vault automatically exits the in\-progress state, and the vault lock policy is removed\. You can call [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) again to install a new vault lock policy and transition into the in\-progress state\.

 The in\-progress state provides the opportunity to test your vault lock policy before you lock it\. Your vault lock policy takes full effect during the in\-progress state just as if the vault has been locked, except that you can remove the policy by calling [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)\. To fine\-tune your policy, you can repeat the [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)/[Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) combination as many times as necessary to validate your vault lock policy changes\.

 After you validate the vault lock policy, you can call [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md) with the most recent lock ID to complete the vault locking process\. Your vault transitions to a locked state where the vault lock policy is unchangeable and can no longer be removed by calling [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)\.

## Related Sections<a name="related-sections-vault-lock-how-to-api"></a>
+ [Amazon S3 Glacier Access Control with Vault Lock Policies](vault-lock-policy.md)
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)