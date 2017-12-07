# Accessing Amazon Glacier<a name="amazon-glacier-accessing"></a>

Amazon Glacier is a RESTful web service that uses HTTP and HTTPS as a transport and JavaScript Object Notation \(JSON\) as a message serialization format\. Your application code can make requests directly to the Amazon Glacier web service API\. When using the REST API directly, you must write the necessary code to sign and authenticate your requests\. For more information about the API, see [API Reference for Amazon Glacier](amazon-glacier-api.md)\. 

Alternatively, you can simplify application development by using the AWS SDKs that wrap the Amazon Glacier REST API calls\. You provide your credentials, and these libraries take care of authentication and request signing\. For more information about using the AWS SDKs, see [Using the AWS SDKs with Amazon Glacier](using-aws-sdk.md)\.

Amazon Glacier also provides a console\. You can use the console to create and delete vaults\. However, all the archive and job operations require you to write code and make requests using either the REST API directly or the AWS SDK wrapper libraries\. To access the Amazon Glacier console, go to [Amazon Glacier Console](https://console.aws.amazon.com/glacier/)\. 

## Regions and Endpoints<a name="regions-and-endpoints-intro"></a>

You create a vault in a specific AWS Region\. You always send your Amazon Glacier requests to a region\-specific endpoint\. For a list of the AWS Regions supported by Amazon Glacier, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#glacier_region) in the *AWS General Reference*\.