# Amazon Glacier Data Retrieval Policies<a name="data-retrieval-policy"></a>

 With Amazon Glacier data retrieval policies, you can easily set data retrieval limits and manage the data retrieval activities across your AWS account in each region\. For more information about Amazon Glacier data retrieval charges, see [Amazon Glacier pricing](https://aws.amazon.com/glacier/pricing/)\.

**Important**  
A data retrieval policy applies to standard retrievals only and manages retrieval requests made directly to Amazon Glacier\. It does not manage data restore requests for the Amazon Simple Storage Service \(Amazon S3\) GLACIER storage class\. For more information about the GLACIER storage class, see [GLACIER Storage Class](http://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html#sc-glacier) and [Transitioning Objects](http://docs.aws.amazon.com/AmazonS3/latest/dev/lifecycle-transition-general-considerations.html) in the *Amazon Simple Storage Service Developer Guide*\. 

**Topics**
+ [Choosing an Amazon Glacier Data Retrieval Policy](#data-retrieval-policy-details)
+ [Using the Amazon Glacier Console to Set Up a Data Retrieval Policy](#data-retrieval-policy-using-console)
+ [Using the Amazon Glacier API to Set Up a Data Retrieval Policy](#data-retrieval-policy-using-api)

## Choosing an Amazon Glacier Data Retrieval Policy<a name="data-retrieval-policy-details"></a>

You can choose from three types of Amazon Glacier data retrieval policies: *Free Tier Only*, *Max Retrieval Rate*, and *No Retrieval Limit*\. By using a Free Tier Only policy, you can keep your retrievals within your daily free tier allowance and not incur any data retrieval cost\. If you want to retrieve more data than the free tier, you can use a Max Retrieval Rate policy to set a bytes\-per\-hour retrieval rate limit\. The Max Retrieval Rate policy ensures that the peak retrieval rate from all retrieval jobs across your account in a region does not exceed the bytes\-per\-hour limit you set\. If you don't want to set a retrieval limit, you can use a No Retrieval Limit policy where all valid data retrieval requests will be accepted\. 

With both Free Tier Only and Max Retrieval Rate policies, data retrieval requests that would exceed the retrieval limits you specified will not be accepted\. If you use a Free Tier Only policy, Amazon Glacier will synchronously reject retrieval requests that would exceed your free tier allowance\. If you use a Max Retrieval Rate policy, Amazon Glacier will reject retrieval requests that would cause the peak retrieval rate of the in progress jobs to exceed the bytes\-per\-hour limit set by the policy\. These policies help you simplify data retrieval cost management\. 

The following are some useful facts about data retrieval policies:
+  Data retrieval policy settings do not change the 3 to 5 hour period that it takes to retrieve data from Amazon Glacier using standard retrievals\.
+ Setting a new data retrieval policy does not affect previously accepted retrieval jobs that are already in progress\. 
+  If a retrieval job request is rejected because of a data retrieval policy, you will not be charged for the job or the request\. 
+ You can set one data retrieval policy for each AWS region, which will govern all data retrieval activities in the region under your account\. A data retrieval policy is region\-specific because data retrieval costs vary across AWS regions\. For more information, see [Amazon Glacier pricing](https://aws.amazon.com/glacier/pricing/)\.

### Free Tier Only Policy<a name="data-retrieval-policy-free-tier-only"></a>

You can set a data retrieval policy to Free Tier Only to ensure that your retrievals will always stay within your free tier allowance, so you don't incur data retrieval charges\. If a retrieval request is rejected, you will receive an error message stating that the request has been denied by the current data retrieval policy\.

You set the data retrieval policy to Free Tier Only for a particular AWS region\. Once the policy is set, you cannot retrieve more data in a day than your prorated daily free retrieval allowance for that region and you will not incur data retrieval fees\. 

You can switch to a Free Tier Only policy after you have incurred data retrieval charges within a month\. The Free Tier Only policy will take effect for new retrieval requests, but will not affect past requests\. You will be billed for the previously incurred charges\. 

### Max Retrieval Rate Policy<a name="data-retrieval-policy-managed-max-retrieval-rate"></a>

You can set your data retrieval policy to Max Retrieval Rate to control the peak retrieval rate by specifying a data retrieval limit that has a bytes\-per\-hour maximum\. When you set the data retrieval policy to Max Retrieval Rate, a new retrieval request will be rejected if it would cause the peak retrieval rate of the in progress jobs to exceed the bytes\-per\-hour limit specified by the policy\. If a retrieval job request is rejected, you will receive an error message stating that the request has been denied by the current data retrieval policy\. 

Setting your data retrieval policy to the Max Retrieval Rate policy can affect how much free tier you can use in a day\. For example, suppose you set Max Retrieval Rate to 1 MB per hour\. This is less than the free tier policy rate of 14 MB per hour\. To ensure you make good use of the daily free tier allowance, you can first set your policy to Free Tier Only and then switch to the Max Retrieval Rate policy later if you need to\. or more information on how your retrieval allowance is calculated, go to [Amazon Glacier FAQs](https://aws.amazon.com/glacier/faqs/)\.

### No Retrieval Limit Policy<a name="data-retrieval-policy-no-retrieval-policy"></a>

 If your data retrieval policy is set to No Retrieval Limit, all valid data retrieval requests will be accepted and your data retrieval costs will vary based on your usage\. 

## Using the Amazon Glacier Console to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-console"></a>

You can view and update the data retrieval policies in the Amazon Glacier console or by using the Amazon Glacier API\. To setup a data retrieval policy in the console, choose an AWS region and then click **Settings**\. 

![\[Data Retrieval Policy dialog\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/gl-data-retrieval-policy.png)![\[Data Retrieval Policy dialog\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Data Retrieval Policy dialog\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

You can select one of the three data retrieval policies: **Free Tier Only**, **Max Retrieval Rate**, or **No Retrieval Limit**\. If you click **Max Retrieval Rate**, you'll need to specify a value in the **GB/Hour** box\. When you type a value in **GB/Hour**, the console will calculate an estimated cost for you\. Click **No Retrieval Limit** if you don't want any restrictions placed on the rate of your data retrievals\. 

You can configure a data retrieval policy for each region\. Each policy will take effect within a few minutes after you click **Save**\. 

## Using the Amazon Glacier API to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-api"></a>

 You can view and set a data retrieval policy by using the Amazon Glacier REST API or by using the AWS SDKs\.

### Using the Amazon Glacier REST API to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-using-api-rest"></a>

You can view and set a data retrieval policy by using the Amazon Glacier REST API\. You can view an existing data retrieval policy by using the [Get Data Retrieval Policy \(GET policy\)](api-GetDataRetrievalPolicy.md) operation\. You set a data retrieval policy using the [Set Data Retrieval Policy \(PUT policy\)](api-SetDataRetrievalPolicy.md) operation\. 

When using the PUT policy operation you select the data retrieval policy type by setting the JSON `Strategy` field value to `BytesPerHour`, `FreeTier`, or `None`\. `BytesPerHour` is equivalent to selecting **Max Retrieval Rate** in the console, `FreeTier` to selecting **Free Tier Only**, and `None` to selecting **No Retrieval Policy**\.

When you use the [Initiate Job \(POST jobs\)](api-initiate-job-post.md) operation to initiate a data retrieval job that will exceed the maximum retrieval rate set in your data retrieval policy, the Initiate Job operation will abort and throw an exception\. 

### Using the AWS SDKs to Set Up a Data Retrieval Policy<a name="data-retrieval-policy-managed-using-api-sdk"></a>

AWS provides SDKs for you to develop applications for Amazon Glacier\. These SDKs provide libraries that map to underlying REST API and provide objects that enable you to easily construct requests and process responses\. For more information, see [Using the AWS SDKs with Amazon Glacier](using-aws-sdk.md)\. 