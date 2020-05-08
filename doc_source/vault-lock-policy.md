# Amazon S3 Glacier Access Control with Vault Lock Policies<a name="vault-lock-policy"></a>

An Amazon S3 Glacier \(S3 Glacier\) vault can have one resource\-based vault access policy and one Vault Lock policy attached to it\. A *Vault Lock policy* is a vault access policy that you can lock\. Using a Vault Lock policy can help you enforce regulatory and compliance requirements\. Amazon S3 Glacier provides a set of API operations for you to manage the Vault Lock policies, see [Locking a Vault by Using the Amazon S3 Glacier API](vault-lock-how-to-api.md)\. 

As an example of a Vault Lock policy, suppose that you are required to retain archives for one year before you can delete them\. To implement this requirement, you can create a Vault Lock policy that denies users permissions to delete an archive until the archive has existed for one year\. You can test this policy before locking it down\. After you lock the policy, the policy becomes immutable\. For more information about the locking process, see [Amazon S3 Glacier Vault Lock](vault-lock.md)\. If you want to manage other user permissions that can be changed, you can use the vault access policy \(see [Amazon S3 Glacier Access Control with Vault Access Policies](vault-access-policy.md)\)\.

You can use the S3 Glacier API, AWS SDKs, AWS CLI, or the S3 Glacier console to create and manage Vault Lock policies\. For a list of S3 Glacier actions allowed for vault resource\-based policies, see [Amazon S3 Glacier API Permissions: Actions, Resources, and Conditions Reference](glacier-api-permissions-ref.md)\.

**Topics**
+ [Example 1: Deny Deletion Permissions for Archives Less Than 365 Days Old](#vault-lock-policy-example-deny-delete-archive-age)
+ [Example 2: Deny Deletion Permissions Based on a Tag](#vault-lock-policy-example-legal-hold-tag)

## Example 1: Deny Deletion Permissions for Archives Less Than 365 Days Old<a name="vault-lock-policy-example-deny-delete-archive-age"></a>

Suppose that you have a regulatory requirement to retain archives for up to one year before you can delete them\. You can enforce that requirement by implementing the following Vault Lock policy\. The policy denies the `glacier:DeleteArchive` action on the examplevault vault if the archive being deleted is less than one year old\. The policy uses the S3 Glacier\-specific condition key `ArchiveAgeInDays` to enforce the one\-year retention requirement\. 

```
 1. {
 2.      "Version":"2012-10-17",
 3.      "Statement":[
 4.       {
 5.          "Sid": "deny-based-on-archive-age",
 6.          "Principal": "*",
 7.          "Effect": "Deny",
 8.          "Action": "glacier:DeleteArchive",
 9.          "Resource": [
10.             "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault"
11.          ],
12.          "Condition": {
13.              "NumericLessThan" : {
14.                   "glacier:ArchiveAgeInDays" : "365"
15.              }
16.          }
17.       }
18.    ]
19. }
```

## Example 2: Deny Deletion Permissions Based on a Tag<a name="vault-lock-policy-example-legal-hold-tag"></a>

Suppose that you have a time\-based retention rule that an archive can be deleted if it is less than a year old\. At the same time, suppose that you need to place a legal hold on your archives to prevent deletion or modification for an indefinite duration during a legal investigation\. In this case, the legal hold takes precedence over the time\-based retention rule specified in the Vault Lock policy\. 

To put these two rules in place, the following example policy has two statements:
+ The first statement denies deletion permissions for everyone, locking the vault\. This lock is performed by using the `LegalHold` tag\.
+ The second statement grants deletion permissions when the archive is less than 365 days old\. But even when archives are less than 365 days old, no one can delete them when the condition in the first statement is met\.

```
 1. {
 2.      "Version":"2012-10-17",
 3.      "Statement":[
 4.       {
 5.         "Sid": "lock-vault",
 6.         "Principal": "*",
 7.         "Effect": "Deny",
 8.         "Action": [
 9.           "glacier:DeleteArchive"
10.         ],
11.         "Resource": [
12.           "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault"
13.         ],
14.         "Condition": {
15.           "StringLike": {
16.             "glacier:ResourceTag/LegalHold": [
17.               "true",
18.               ""
19.             ]
20.           }
21.         }
22.       },
23.       {
24.         "Sid": "you-can-delete-archive-less-than-1-year-old",
25.         "Principal": {
26.             "AWS": "arn:aws:iam::123456789012:root"
27.           },
28.         "Effect": "Allow",
29.         "Action": [
30.           "glacier:DeleteArchive"
31.         ],
32.         "Resource": [
33.           "arn:aws:glacier:us-west-2:123456789012:vaults/examplevault"
34.         ],
35.         "Condition": {
36.           "NumericLessThan": {
37.             "glacier:ArchiveAgeInDays": "365"
38.           }
39.         }
40.       }
41.    ]
42. }
```

### Related Sections<a name="related-sections-vault-lock-policy-examples"></a>
+ [Amazon S3 Glacier Vault Lock](vault-lock.md)
+ [Abort Vault Lock \(DELETE lock\-policy\)](api-AbortVaultLock.md)
+ [Complete Vault Lock \(POST lockId\)](api-CompleteVaultLock.md)
+ [Get Vault Lock \(GET lock\-policy\)](api-GetVaultLock.md)
+ [Initiate Vault Lock \(POST lock\-policy\)](api-InitiateVaultLock.md)