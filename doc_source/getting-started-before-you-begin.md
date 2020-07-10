# Step 1: Before You Begin with Amazon S3 Glacier<a name="getting-started-before-you-begin"></a>

Before you can start with this exercise, you must sign up for an AWS account \(if you don't already have one\), and then download one of the AWS Software Development Kits \(SDKs\)\. The following sections provide instructions\.

**Topics**
+ [Set Up an AWS Account and an Administrator User](#setup)
+ [Download the Appropriate AWS SDK](#getting-started-download-sdk)

**Important**  
Amazon S3 Glacier \(S3 Glacier\) provides a management console, which you can use to create and delete vaults\. However, all other interactions with S3 Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using S3 Glacier with the AWS CLI, go to [AWS CLI Reference for S3 Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

## Set Up an AWS Account and an Administrator User<a name="setup"></a>

If you have not already done so, you need to sign up for an AWS account and create an administrator user in the account\. 

To complete the setup, follow the instructions in the following topics:

### Set Up an AWS Account and Create an Administrator User<a name="setting-up"></a>

#### Sign up for AWS<a name="setting-up-signup"></a>

When you sign up for Amazon Web Services \(AWS\), your AWS account is automatically signed up for all services in AWS, including S3 Glacier\. You are charged only for the services that you use\. For more information about S3 Glacier usage rates, see the [Amazon S3 Glacier product page](https://aws.amazon.com/glacier/)\.

If you already have an AWS account and you have created an IAM user for the account, skip to the next task\. If you don't have an AWS account, use the following procedure to create one\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

Note your AWS account ID, because you'll need it for the next step\.

#### Create an IAM User<a name="setting-up-iam"></a>

Services in AWS, such as S3 Glacier, require that you provide credentials when you access them, so that the service can determine whether you have permissions to access the resources owned by that service\. The console requires your password\. You can create access keys for your AWS account to access the AWS CLI or API\. However, we don't recommend that you access AWS using the credentials for your AWS account\. Instead, we recommend that you use AWS Identity and Access Management \(IAM\)\. Create an IAM user, add the user to an IAM group with administrative permissions, and then grant administrative permissions to the IAM user that you created\. You can then access AWS using a special URL and that IAM user's credentials\.

If you signed up for AWS, but you haven't created an IAM user for yourself, you can create one using the IAM console\.

The Getting Started examples in this guide assume you have a user with administrator privileges\.

**To create an administrator user for yourself and add the user to an administrators group \(console\)**

1. Sign in to the [IAM console](https://console.aws.amazon.com/iam/) as the account owner by choosing **Root user** and entering your AWS account email address\. On the next page, enter your password\.
**Note**  
We strongly recommend that you adhere to the best practice of using the **Administrator** IAM user below and securely lock away the root user credentials\. Sign in as the root user only to perform a few [account and service management tasks](https://docs.aws.amazon.com/general/latest/gr/aws_tasks-that-require-root.html)\.

1. In the navigation pane, choose **Users** and then choose **Add user**\.

1. For **User name**, enter **Administrator**\.

1. Select the check box next to **AWS Management Console access**\. Then select **Custom password**, and then enter your new password in the text box\.

1. \(Optional\) By default, AWS requires the new user to create a new password when first signing in\. You can clear the check box next to **User must create a new password at next sign\-in** to allow the new user to reset their password after they sign in\.

1. Choose **Next: Permissions**\.

1. Under **Set permissions**, choose **Add user to group**\.

1. Choose **Create group**\.

1. In the **Create group** dialog box, for **Group name** enter **Administrators**\.

1. Choose **Filter policies**, and then select **AWS managed \-job function** to filter the table contents\.

1. In the policy list, select the check box for **AdministratorAccess**\. Then choose **Create group**\.
**Note**  
You must activate IAM user and role access to Billing before you can use the `AdministratorAccess` permissions to access the AWS Billing and Cost Management console\. To do this, follow the instructions in [step 1 of the tutorial about delegating access to the billing console](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_billing.html)\.

1. Back in the list of groups, select the check box for your new group\. Choose **Refresh** if necessary to see the group in the list\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add metadata to the user by attaching tags as key\-value pairs\. For more information about using tags in IAM, see [Tagging IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\.

1. Choose **Next: Review** to see the list of group memberships to be added to the new user\. When you are ready to proceed, choose **Create user**\.

You can use this same process to create more groups and users and to give your users access to your AWS account resources\. To learn about using policies that restrict user permissions to specific AWS resources, see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Example Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_examples.html)\.

**To sign in as the new IAM user**

1. Sign out of the AWS Management Console\.

1. Use the following URL format to log in to the console:

   ```
   https://aws_account_number.signin.aws.amazon.com/console/
   ```

   The *aws\_account\_number* is your AWS account ID without hyphen\. For example, if your AWS account ID is `1234-5678-9012`, your AWS account number is `123456789012`\. For information about how to find your account number, see [Your AWS Account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

1. Enter the IAM user name and password that you just created\. When you're signed in, the navigation bar displays *your\_user\_name* @ *your\_aws\_account\_id*\.

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
+ [Getting Started](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)

For information about using IAM with S3 Glacier, see [Identity and Access Management in Amazon S3 Glacier](auth-and-access-control.md)\.

## Download the Appropriate AWS SDK<a name="getting-started-download-sdk"></a>

To try the getting started exercise, you must decide which programming language you want to use and download the appropriate AWS SDK for your development platform\.

The getting started exercise provides examples in Java and C\#\. 

### Downloading the AWS SDK for Java<a name="getting-started-download-sdk-java"></a>

To test the Java examples in this developer guide, you need the AWS SDK for Java\. You have the following download options: 
+ If you are using Eclipse, you can download and install the AWS Toolkit for Eclipse using the update site [http://aws\.amazon\.com/eclipse/](http://aws.amazon.com/eclipse/)\. For more information, go to [AWS Toolkit for Eclipse](http://aws.amazon.com/eclipse/)\.
+ If you are using any other IDE to create your application, download the [AWS SDK for Java](http://aws.amazon.com/sdkforjava)\. 

### Downloading the AWS SDK for \.NET<a name="getting-started-download-sdk-dotnet"></a>

To test the C\# examples in this developer guide, you need the AWS SDK for \.NET\. You have the following download options:
+ If you are using Visual Studio, you can install both the AWS SDK for \.NET and the AWS Toolkit for Visual Studio\. The toolkit provides AWS Explorer for Visual Studio and project templates that you can use for development\. To download the AWS SDK for \.NET go to [http://aws\.amazon\.com/sdkfornet](http://aws.amazon.com/sdkfornet/)\. By default, the installation script installs both the AWS SDK and the AWS Toolkit for Visual Studio\. To learn more about the toolkit, go to [AWS Toolkit for Visual Studio User Guide](https://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/)\. 
+ If you are using any other IDE to create your application, you can use the same link provided in the preceding step and install only the AWS SDK for \.NET\. 