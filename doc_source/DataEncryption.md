# Data Encryption<a name="DataEncryption"></a>

Data protection refers to protecting data while in\-transit \(as it travels to and from Amazon S3 Glacier\) and at rest \(while it is stored in AWS data centers\)\. You can protect data in transit that is uploaded directly to S3 Glacier using Secure Sockets Layer \(SSL\) or client\-side encryption\.

You can also access S3 Glacier through Amazon S3\. Amazon S3 supports lifecycle configuration on an Amazon S3 bucket, which enables you to transition objects to the S3 Glacier storage class for archival\. Data in transit between Amazon S3 and S3 Glacier via lifecycle policies is encrypted using SSL\.

Data at rest stored in S3 Glacier is automatically server\-side encrypted using 256\-bit Advanced Encryption Standard \(AES\-256\) with keys maintained by AWS\. If you prefer to manage your own keys, you can also use client\-side encryption before storing data in S3 Glacier\. For more information about how to setup default encryption for Amazon S3, see [Amazon S3 Default Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html) in the *Amazon Simple Storage Service User Guide*\.