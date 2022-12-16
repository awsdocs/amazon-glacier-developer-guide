# Step 1: Before You Begin with S3 Glacier<a name="getting-started-before-you-begin"></a>

Before you can start with this exercise, you must sign up for an AWS account \(if you don't already have one\), and then download one of the AWS SDKs\. The following sections provide instructions\.

**Topics**
+ [Set Up an AWS account and an Administrator User](#setup)
+ [Download the Appropriate AWS SDK](#getting-started-download-sdk)

**Important**  
S3 Glacier does provide a console\. However, any archive operation, such as upload, download, or deletion, requires that you use the AWS Command Line Interface \(CLI\) or write code\. There is no console support for archive operations\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, by using either the REST API directly or by using the AWS SDKs\.   
To install the AWS CLI, see [AWS Command Line Interface](http://aws.amazon.com/cli/)\. For more information about using S3 Glacier with the AWS CLI, see the [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. For examples of using the AWS CLI to upload archives to S3 Glacier, see [Using S3 Glacier with the AWS Command Line Interface](http://docs.aws.amazon.com/cli/latest/userguide/cli-using-glacier.html)\. 

## Set Up an AWS account and an Administrator User<a name="setup"></a>

If you have not already done so, you must sign up for an AWS account and create an administrator user in the account\. 

To complete the setup, follow the instructions in the following topics\.

### Set Up an AWS account and Create an Administrator User<a name="setting-up"></a>

#### Sign up for AWS<a name="setting-up-signup"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including S3 Glacier\. You are charged only for the services that you use\. For more information about S3 Glacier usage rates, see the [Amazon S3 Glacier product page](https://aws.amazon.com/glacier/)\.

If you already have an AWS account and you have created an AWS Identity and Access Management \(IAM\) user for the account, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To sign up for an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

   When you sign up for an AWS account, an *AWS account root user* is created\. The root user has access to all AWS services and resources in the account\. As a security best practice, [assign administrative access to an administrative user](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html), and use only the root user to perform [tasks that require root user access](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html)\.

AWS sends you a confirmation email after the sign\-up process is complete\. At any time, you can view your current account activity and manage your account by going to [https://aws\.amazon\.com/](https://aws.amazon.com/) and choosing **My Account**\.

#### Create an IAM User<a name="setting-up-iam"></a>

Services in AWS, such as S3 Glacier, require that you provide credentials when you access them, so that the service can determine whether you have permissions to access the resources owned by that service\. The console requires your password\. You can create access keys for your AWS account to access the AWS CLI or API\. However, we don't recommend that you access AWS using the credentials for your AWS account\. Instead, we recommend that you use AWS Identity and Access Management \(IAM\)\. Create an IAM user, add the user to an IAM group with administrative permissions, and then grant administrative permissions to the IAM user that you created\. You can then access AWS using a special URL and that IAM user's credentials\.

If you signed up for AWS, but you haven't created an IAM user for yourself, you can create one by using the IAM console\.

The Getting Started exercise in this guide assumes that you have a user with administrator privileges\.



To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

**To sign in as the new IAM user**

1. Sign out of the AWS Management Console\.

1. Use the following URL format to log in to the console:

   ```
   https://aws_account_number.signin.aws.amazon.com/console/
   ```

   The `aws_account_number` is your AWS account ID without hyphen\. For example, if your AWS account ID is `1234-5678-9012`, your AWS account number is `123456789012`\. For information about how to find your account number, see [Your AWS account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

1. Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays `your_user_name` `@` `your_aws_account_id`\.

If you don't want the URL for your sign\-in page to contain your AWS account ID, you can create an account alias\. 

**To create or remove an account alias**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. On the navigation pane, choose **Dashboard**\.

1. Find the IAM users sign\-in link\.

1. To create the alias, click **Customize**, enter the name you want to use for your alias, and then choose **Yes, Create**\.

1. To remove the alias, choose **Customize**, and then choose **Yes, Delete**\. The sign\-in URL reverts to using your AWS account ID\.

To sign in after you create an account alias, use the following URL:

```
https://your_account_alias.signin.aws.amazon.com/console/
```

To verify the sign\-in link for IAM users for your account, open the IAM console and check under **IAM users sign\-in link:** on the dashboard\.

For more information about IAM, see the following:
+ [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)
+ [Getting started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)

For information about using IAM with S3 Glacier, see [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)\.

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