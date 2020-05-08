# SelectParameters<a name="api-SelectParameters"></a>

Contains information about the parameters used for the select\.

## Contents<a name="api-SelectParameters-contents"></a>

**Expression**  
The expression that is used to select the object\. The expression must not exceed the quota of 128,000 characters\.  
*Type*: String  
*Required*: yes

**ExpressionType**  
The type of the provided expression, for example `SQL`\.  
*Valid Values*: `SQL`  
*Type*: String  
*Required*: yes

**InputSerialization**  
Describes the serialization format of the object in the select\.  
*Type*: [InputSerialization](api-InputSerialization.md) object  
*Required*: no

**OutputSerialization**  
Describes how the results of the select job are serialized\.  
*Required*: no  
*Type*: [OutputSerialization](api-OutputSerialization.md) object

## More Info<a name="more-info-api-SelectParameters"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)