# CSVOutput<a name="api-CSVOutput"></a>

Contains information about the comma\-separated values \(CSV\) format that the job results are stored in\.

## Contents<a name="api-CSVOutput-contents"></a>

**FieldDelimiter**  
A single character used to separate individual fields from each other within a record\.  
*Type*: String  
*Required*: no

**QuoteCharacter**  
A single character used as an escape character where the field delimiter is part of the value\.  
*Type*: String  
*Required*: no

**QuoteEscapeCharacter**  
A single character used for escaping the quotation\-mark character inside an already escaped value\.  
*Type*: String  
*Required*: no

**QuoteFields**  
A value that indicates whether all output fields should be contained within quotation marks\.  
*Valid Values*: `ALWAYS` \| `ASNEEDED`  
*Type*: String  
*Required*: no

**RecordDelimiter**  
A single character used to separate individual records from each other\.  
*Type*: String  
*Required*: no

## More Info<a name="more-info-api-CSVOutput"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)