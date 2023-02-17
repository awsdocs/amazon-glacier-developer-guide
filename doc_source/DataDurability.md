# Data Protection in Amazon S3 Glacier<a name="DataDurability"></a>

Amazon S3 Glacier \(S3 Glacier\) provides highly durable cloud storage for data archiving and long\-term backup\. S3 Glacier is designed to deliver 99\.999999999 percent durability and provides comprehensive security and compliance capabilities that can help you meet stringent regulatory requirements\. S3 Glacier redundantly stores data in multiple AWS Availability Zones \(AZ\) and on multiple devices within each AZ\. To increase durability, S3 Glacier synchronously stores your data across multiple AZs before confirming a successful upload\.

For more information about the AWS global cloud infrastructure, see [Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)\.

For data protection purposes, we recommend that you protect AWS account credentials and give individual users, groups, or roles only the permissions necessary to fulfill their job duties\.

If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

**Topics**
+ [Data Encryption](DataEncryption.md)
+ [Key Management](key-management.md)
+ [Internetwork Traffic Privacy](InternetworkTrafficPrivacy.md)