# S3 Glacier Data Retrieval Policies<a name="data-retrieval-policy"></a>

 With Amazon S3 Glacier data retrieval policies, you can easily set data retrieval quotas and manage the data retrieval activities across your AWS account in each AWS Region\. For more information about S3 Glacier data retrieval charges, see [S3 Glacier pricing](https://aws.amazon.com/s3/glacier/pricing/)\.

**Important**  
A data retrieval policy applies only to Standard retrievals and manages retrieval requests made directly to S3 Glacier\.   
For more information about the S3 Glacier storage classes, see [Storage classes for archiving objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) and [Transitioning objects](https://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service User Guide*\. 

**Topics**
+ [Choosing an S3 Glacier Data Retrieval Policy](#data-retrieval-policy-details)
+ [Using the S3 Glacier Console to Set Up a Data Retrieval Policy](#data-retrieval-policy-using-console)
+ [Using the Amazon S3 Glacier API to Set Up a Data Retrieval Policy](#data-retrieval-policy-using-api)

## Choosing an S3 Glacier Data Retrieval Policy<a name="data-retrieval-policy-details"></a>

You can choose from three types of S3 Glacier data retrieval policies: No Retrieval Limit, Free Tier Only, and Max Retrieval Rate\. 

No Retrieval Limit is the default data retrieval policy that's used for retrievals\. If you use the No Retrieval Limit policy, no retrieval quota is set, and all valid data retrieval requests are accepted\. 

By using a Free Tier Only policy, you can keep your retrievals within your daily AWS Free Tier allowance and not incur any data retrieval costs\. If you want to retrieve more data than is in your AWS Free Tier allowance, you can use a Max Retrieval Rate policy to set a bytes\-per\-hour retrieval\-rate quota\. The Max Retrieval Rate policy ensures that the peak retrieval rate from all retrieval jobs across your account in an AWS Region does not exceed the bytes\-per\-hour quota that you set\. 

With both the Free Tier Only and Max Retrieval Rate policies, data retrieval requests that exceed the retrieval quotas that you specified are not accepted\. If you use a Free Tier Only policy, S3 Glacier synchronously rejects retrieval requests that exceed your AWS Free Tier allowance\. If you use a Max Retrieval Rate policy, S3 Glacier rejects retrieval requests that cause the peak retrieval rate of the in\-progress jobs to exceed the bytes\-per\-hour quota set by the policy\. These policies help you simplify data retrieval cost management\. 

The following are some useful facts about data retrieval policies:
+ Data retrieval policy settings do not change the 3\- to 5\-hour period that it takes to retrieve data from S3 Glacier by using Standard retrievals\.
+ Setting a new data retrieval policy does not affect previously accepted retrieval jobs that are already in progress\. 
+ If a retrieval job request is rejected because of a data retrieval policy, you are not charged for the job or the request\. 
+ You can set one data retrieval policy for each AWS Region, which will govern all data retrieval activities in the AWS Region under your account\. A data retrieval policy is specific to a particular AWS Region because data retrieval costs vary across AWS Regions\. For more information, see [Amazon S3 Glacier pricing](https://aws.amazon.com/s3/glacier/pricing/)\.

### Free Tier Only Policy<a name="data-retrieval-policy-free-tier-only"></a>

You can set a data retrieval policy to Free Tier Only to ensure that your retrievals always stay within your AWS Free Tier allowance, so that you don't incur data retrieval charges\. If a retrieval request is rejected, you receive an error message stating that the request has been denied by the current data retrieval policy\.

You can set the data retrieval policy to Free Tier Only on a per\-Region basis\. After the policy is set, you cannot retrieve more data in a day than your prorated daily AWS Free Tier retrieval allowance for that AWS Region\. You also do not incur data retrieval fees\. 

You can also switch to a Free Tier Only policy after you have incurred data retrieval charges within a month\. In that case, the Free Tier Only policy takes effect for new retrieval requests, but does not affect past requests\. You will be billed for the previously incurred charges\. 

### Max Retrieval Rate Policy<a name="data-retrieval-policy-managed-max-retrieval-rate"></a>

You can set your data retrieval policy to Max Retrieval Rate to control the peak retrieval rate by specifying a data retrieval quota that has a bytes\-per\-hour maximum\. When you set the data retrieval policy to Max Retrieval Rate, a new retrieval request is rejected if it would cause the peak retrieval rate of the in\-progress jobs to exceed the bytes\-per\-hour quota that's specified by the policy\. If a retrieval job request is rejected, you receive an error message stating that the request has been denied by the current data retrieval policy\. 

Setting your data retrieval policy to the Max Retrieval Rate policy can affect how much of your AWS Free Tier allowance that you can use in a day\. For example, suppose you set Max Retrieval Rate to 1 MB per hour\. This is less than the AWS Free Tier policy rate\. To ensure that you make good use of the daily AWS Free Tier allowance, you can first set your policy to Free Tier Only, and then switch to the Max Retrieval Rate policy later if you need to\. For more information about how your retrieval allowance is calculated, go to [Amazon S3 Glacier FAQs](https://aws.amazon.com/glacier/faqs/)\.

### No Retrieval Limit Policy<a name="data-retrieval-policy-no-retrieval-policy"></a>

If your data retrieval policy is set to No Retrieval Limit, all valid data retrieval requests are accepted and your data retrieval costs will vary based on your usage\. 

## Using the S3 Glacier Console to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-console"></a>

**To create a data retrieval policy by using the Amazon S3 Glacier console**

1.  Sign in to the AWS Management Console and open the S3 Glacier console at [https://console\.aws\.amazon\.com/glacier/home](https://console.aws.amazon.com/glacier/home)\.

1. Under **Select a Region**, choose an AWS Region from the dropdown menu\. You can configure a data retrieval policy for each AWS Region\.

1. In the left navigation pane, choose **Data retrieval settings**\.

1. Choose **Edit**\. The **Edit data retrieval policies** page appears\.

1. Under **Data retrieval policies**, choose a policy\.

   You can select one of the three data retrieval policies: **No retrieval limit**, **Free Tier only**, or **Specify a max retrieval rate**\.
   + If you choose **No retrieval limit**, all valid data retrieval requests are accepted\. 
   + If you choose **Free Tier only**, data retrieval requests that exceed the AWS Free Tier are not accepted\. 
   + If you choose **Specify a max retrieval rate**, data retrieval requests are rejected if they would cause the peak retrieval rate of the in\-progress jobs to exceed the max retrieval rate that you specify\. You must specify a gigabytes \(GB\) per hour value in the **GB/hour** box under **Max retrieval rate**\. When you enter a value for **GB/hour**, the console calculates an estimated cost for you\. 

1.  Choose **Save changes**\. 

## Using the Amazon S3 Glacier API to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-api"></a>

 You can view and set a data retrieval policy by using the Amazon S3 Glacier REST API or by using the AWS SDKs\.

### Using the Amazon S3 Glacier REST API to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-api-rest"></a>

You can view and set a data retrieval policy by using the Amazon S3 Glacier REST API\. You can view an existing data retrieval policy by using the [Get Data Retrieval Policy \(GET policy\)](api-GetDataRetrievalPolicy.md) operation\. You set a data retrieval policy by using the [Set Data Retrieval Policy \(PUT policy\)](api-SetDataRetrievalPolicy.md) operation\. 

When using the `PUT` policy operation, you select the data retrieval policy type by setting the JSON `Strategy` field value to `BytesPerHour`, `FreeTier`, or `None`\. `BytesPerHour` is equivalent to choosing **Specify a max retrieval rate** in the console, `FreeTier` to choosing **Free Tier only**, and `None` to choosing **No retrieval limit**\.

When you use the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) operation to initiate a data retrieval job that will exceed the maximum retrieval rate set in your data retrieval policy, the `Initiate Job` operation stops and throws an exception\. 

### Using the AWS SDKs to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-managed-using-api-sdk"></a>

AWS provides SDKs for you to develop applications for Amazon S3 Glacier\. These SDKs provide libraries that map to the underlying REST API and provide objects that enable you to easily construct requests and process responses\. For more information, see [Using the AWS SDKs with Amazon S3 Glacier](using-aws-sdk.md)\. 