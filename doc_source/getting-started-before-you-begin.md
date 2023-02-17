# Step 1: Before You Begin with S3 Glacier<a name="getting-started-before-you-begin"></a>

Before you can start with this exercise, you must sign up for an AWS account \(if you don't already have one\), and then download one of the AWS SDKs\. See the following sections for instructions\.

**Topics**
+ [Set Up an AWS account and an Administrator User](#setup)
+ [Download the Appropriate AWS SDK](#getting-started-download-sdk)

## Set Up an AWS account and an Administrator User<a name="setup"></a>

If you have not already done so, you must sign up for an AWS account and create an administrator user in the account\. 

To complete the setup, follow the instructions in the following topics\.

### Set Up an AWS account and Create an Administrator User<a name="setting-up"></a>

#### Sign up for AWS<a name="setting-up-signup"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including S3 Glacier\. You are charged only for the services that you use\. For more information about S3 Glacier usage rates, see the [Amazon S3 Glacier product page](https://aws.amazon.com/glacier/)\.

If you already have an AWS account, skip to [Download the Appropriate AWS SDK](#getting-started-download-sdk)\. If you don't have an AWS account, use the following procedure to create one\.

If you do not have an AWS account, complete the following steps to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

## Download the Appropriate AWS SDK<a name="getting-started-download-sdk"></a>

To try the getting started exercise, you must decide which programming language you want to use, and then download the appropriate AWS SDK for your development platform\.

The getting started exercise provides examples in Java and C\#\. 

### Downloading the AWS SDK for Java<a name="getting-started-download-sdk-java"></a>

To test the Java examples in this developer guide, you need the AWS SDK for Java\. You have the following download options: 
+ If you are using Eclipse, you can download and install the AWS Toolkit for Eclipse by using the update site [http://aws\.amazon\.com/eclipse/](http://aws.amazon.com/eclipse/)\. For more information, see [AWS Toolkit for Eclipse](http://aws.amazon.com/eclipse/)\.
+ If you are using any other IDE to create your application, download the [AWS SDK for Java](http://aws.amazon.com/sdkforjava)\. 

### Downloading the AWS SDK for \.NET<a name="getting-started-download-sdk-dotnet"></a>

To test the C\# examples in this developer guide, you need the AWS SDK for \.NET\. You have the following download options:
+ If you are using Visual Studio, you can install both the AWS SDK for \.NET and the AWS Toolkit for Visual Studio\. The toolkit provides AWS Explorer for Visual Studio and project templates that you can use for development\. To download the AWS SDK for \.NET, go to [http://aws\.amazon\.com/sdkfornet](http://aws.amazon.com/sdkfornet/)\. By default, the installation script installs both the AWS SDK and the AWS Toolkit for Visual Studio\. To learn more about the toolkit, see the [AWS Toolkit for Visual Studio User Guide](https://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/)\. 
+ If you are using any other IDE to create your application, you can use the same link provided in the preceding step and install only the AWS SDK for \.NET\. 