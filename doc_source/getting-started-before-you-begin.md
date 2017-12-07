# Step 1: Before You Begin with Amazon Glacier<a name="getting-started-before-you-begin"></a>

Before you can start with this exercise, you must sign up for an AWS account \(if you don't already have one\), and then download one of the AWS Software Development Kits \(SDKs\)\. The following sections provide instructions\.

**Important**  
Amazon Glacier provides a management console, which you can use to create and delete vaults\. However, all other interactions with Amazon Glacier require that you use the AWS Command Line Interface \(CLI\) or write code\. For example, to upload data, such as photos, videos, and other documents, you must either use the AWS CLI or write code to make requests, using either the REST API directly or by using the AWS SDKs\. For more information about using Amazon Glacier with the AWS CLI, go to [AWS CLI Reference for Amazon Glacier](http://docs.aws.amazon.com/cli/latest/reference/glacier/index.html)\. To install the AWS CLI, go to [AWS Command Line Interface](http://aws.amazon.com/cli/)\.

## Sign Up<a name="getting-started-sign-up"></a>

If you already have an AWS account, go ahead and skip to the next section: [Download the Appropriate AWS SDK](#getting-started-download-sdk)\. Otherwise, follow these steps\.

**To sign up for an AWS account**

1. Open [https://aws\.amazon\.com/](https://aws.amazon.com/), and then choose **Create an AWS Account**\.
**Note**  
This might be unavailable in your browser if you previously signed into the AWS Management Console\. In that case, choose **Sign In to the Console**, and then choose **Create a new AWS account**\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a PIN using the phone keypad\.

## Download the Appropriate AWS SDK<a name="getting-started-download-sdk"></a>

To try the getting started exercise, you must decide which programming language you want to use and download the appropriate AWS SDK for your development platform\.

The getting started exercise provides examples in Java and C\#\. 

### Downloading the AWS SDK for Java<a name="getting-started-download-sdk-java"></a>

To test the Java examples in this developer guide, you need the AWS SDK for Java\. You have the following download options: 

+ If you are using Eclipse, you can download and install the AWS Toolkit for Eclipse using the update site [http://aws\.amazon\.com/eclipse/](http://aws.amazon.com/eclipse/)\. For more information, go to [AWS Toolkit for Eclipse](http://aws.amazon.com/eclipse/)\.

+ If you are using any other IDE to create your application, download the [ AWS SDK for Java ](http://aws.amazon.com/sdkforjava)\. 

### Downloading the AWS SDK for \.NET<a name="getting-started-download-sdk-dotnet"></a>

To test the C\# examples in this developer guide, you need the AWS SDK for \.NET\. You have the following download options:

+ If you are using Visual Studio, you can install both the AWS SDK for \.NET and the AWS Toolkit for Visual Studio\. The toolkit provides AWS Explorer for Visual Studio and project templates that you can use for development\. Go to [http://aws\.amazon\.com/sdkfornet](http://aws.amazon.com/sdkfornet/) and click **Download AWS SDK for \.NET**\. By default, the installation script installs both the AWS SDK and the AWS Toolkit for Visual Studio\. To learn more about the toolkit, go to [AWS Toolkit for Visual Studio User Guide](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/)\. 

+ If you are using any other IDE to create your application, you can use the same link provided in the preceding step and install only the AWS SDK for \.NET\. 