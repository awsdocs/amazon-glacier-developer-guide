# Locking a Vault by Using the S3 Glacier API<a name="vault-lock-how-to-api"></a>

To lock your vault with the Amazon S3 Glacier API, you first call [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md) with a Vault Lock policy that specifies the controls that you want to deploy\. The `Initiate Vault Lock` operation attaches the policy to your vault, transitions the Vault Lock to the in\-progress state, and returns a unique lock ID\. After the Vault Lock enters the in\-progress state, you have 24 hours to complete the lock by calling [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md) with the lock ID that was returned from the `Initiate Vault Lock` call\. 

**Important**  
We recommend that you first create a vault, complete a Vault Lock policy, and then upload your archives to the vault so that the policy will be applied to them\.
After the Vault Lock policy is locked, it cannot be changed or deleted\.

If you don't complete the Vault Lock process within 24 hours after entering the in\-progress state, your vault automatically exits the in\-progress state, and the Vault Lock policy is removed\. You can call `Initiate Vault Lock` again to install a new Vault Lock policy and transition into the in\-progress state\.

The in\-progress state provides the opportunity to test your Vault Lock policy before you lock it\. Your Vault Lock policy takes full effect during the in\-progress state just as if the vault has been locked, except that you can remove the policy by calling [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)\. To fine\-tune your policy, you can repeat the `Abort Vault Lock`/`Initiate Vault Lock` combination as many times as necessary to validate your Vault Lock policy changes\.

After you validate the Vault Lock policy, you can call [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md) with the most recent lock ID to complete the vault locking process\. Your vault transitions to a locked state, where the Vault Lock policy is unchangeable and can no longer be removed by calling `Abort Vault Lock`\.

## Related Sections<a name="related-sections-vault-lock-how-to-api"></a>

 
+ [Vault Lock Policies](vault-lock-policy.md)
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)