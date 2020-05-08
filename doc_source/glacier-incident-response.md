# Logging and Monitoring in Amazon S3 Glacier<a name="glacier-incident-response"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon S3 Glacier \(S3 Glacier\) and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily identify and debug the source of a failure if one occurs\. AWS provides the following tools for monitoring your S3 Glacier resources and responding to potential incidents:

**Amazon CloudWatch Alarms**  
When using S3 Glacier via Amazon S3, you can use Amazon CloudWatch alarms to watch a single metric over a time period that you specify\. If the metric exceeds a given threshold, a notification is sent to an Amazon SNS topic or AWS Auto Scaling policy\. CloudWatch alarms do not invoke actions because they are in a particular state\. Rather the state must have changed and been maintained for a specified number of periods\. For more information, see [Monitoring Metrics with Amazon CloudWatch](https://docs.aws.amazon.com/AmazonS3/latest/dev/cloudwatch-monitoring.html)\.

**AWS CloudTrail Logs**  
CloudTrail provides a record of actions taken by a user, role, or an AWS service in S3 Glacier\. CloudTrail captures all API calls for S3 Glacier as events, including calls from the S3 Glacier console and from code calls to the S3 Glacier APIs\. For more information, see [Logging Amazon S3 Glacier API Calls with AWS CloudTrail](audit-logging.md)\.

**AWS Trusted Advisor**  
Trusted Advisor draws upon best practices learned from serving hundreds of thousands of AWS customers\. Trusted Advisor inspects your AWS environment and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps\. All AWS customers have access to five Trusted Advisor checks\. Customers with a Business or Enterprise support plan can view all Trusted Advisor checks\.   
For more information, see [AWS Trusted Advisor](https://docs.aws.amazon.com/awssupport/latest/user/getting-started.html#trusted-advisor) in the *AWS Support User Guide*\.