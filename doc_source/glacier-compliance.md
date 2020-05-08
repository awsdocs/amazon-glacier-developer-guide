# Compliance Validation for Amazon S3 Glacier<a name="glacier-compliance"></a>

The security and compliance of Amazon S3 Glacier \(S3 Glacier\) is assessed by third\-party auditors as part of multiple AWS compliance programs, including the following:
+ System and Organization Controls \(SOC\)
+ Payment Card Industry Data Security Standard \(PCI DSS\)
+ Federal Risk and Authorization Management Program \(FedRAMP\)
+ Health Insurance Portability and Accountability Act \(HIPAA\)

AWS provides a frequently updated list of AWS services in scope of specific compliance programs at [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)\. 

Third\-party audit reports are available for you to download using AWS Artifact\. For more information, see [Downloading Reports in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/downloading-documents.html) in the *AWS Artifact User Guide*\. 

For more information about AWS compliance programs, see [AWS Compliance Programs](https://aws.amazon.com/compliance/programs/)\.

Your compliance responsibility when using S3 Glacier is determined by the sensitivity of your data, your organization’s compliance objectives, and applicable laws and regulations\. If your use of S3 Glacier is subject to compliance with standards like HIPAA, PCI, or FedRAMP, AWS provides resources to help:
+ [Amazon S3 Glacier Vault Lock](vault-lock.md) allows you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a vault lock policy\. You can specify controls such as “write once read many” \(WORM\) in a vault lock policy and lock the policy from future edits\. After the policy is locked, it can no longer be changed\. Vault lock policies can help you comply with regulatory frameworks such as SEC17a\-4 and HIPAA\.
+ [Security and Compliance Quick Start Guides](https://aws.amazon.com/quickstart/?awsf.quickstart-homepage-filter=categories%23security-identity-compliance) discuss architectural considerations and steps for deploying security\- and compliance\-focused baseline environments on AWS\. 
+ [Architecting for HIPAA Security and Compliance](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf) whitepaper outlines how companies use AWS to help them meet HIPAA requirements\.
+ [The AWS Well\-Architected Tool \(AWS WA Tool\)](https://docs.aws.amazon.com/wellarchitected/latest/userguide/intro.html) is a service in the cloud that provides a consistent process for you to review and measure your architecture using AWS best practices\. The AWS WA Tool provides recommendations for making your workloads more reliable, secure, efficient, and cost\-effective\.
+ [AWS Compliance Resources](https://aws.amazon.com/compliance/resources/) provide several different workbooks and guides that might apply to your industry and location\.
+ [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) can help you assess how well your resource configurations comply with internal practices, industry guidelines, and regulations\.
+ [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) provides you with a comprehensive view of your security state within AWS and helps you check your compliance with security industry standards and best practices\. 