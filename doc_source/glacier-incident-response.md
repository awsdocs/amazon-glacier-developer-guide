# Logging and Monitoring in Amazon S3 Glacier<a name="glacier-incident-response"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon S3 Glacier \(Glacier\) and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides the following tools for monitoring your Glacier resources and responding to potential incidents:

**AWS CloudTrail Logs**  
CloudTrail provides a record of actions taken by a user, role, or an AWS service in Glacier\. CloudTrail captures all API calls for Glacier as events, including calls from the Glacier console and from code calls to the Glacier APIs\. For more information, see [Logging Amazon S3 Glacier API Calls with AWS CloudTrail](audit-logging.md)\.

**AWS Trusted Advisor**  
Trusted Advisor draws upon best practices learned from serving hundreds of thousands of AWS customers\. Trusted Advisor inspects your AWS environment and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps\. All AWS customers have access to five Trusted Advisor checks\. Customers with a Business or Enterprise support plan can view all Trusted Advisor checks\.   
For more information, see [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html#trusted-advisor) in the *AWS Support User Guide*\.