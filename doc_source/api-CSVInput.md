# CSVInput<a name="api-CSVInput"></a>

Contains information about the comma\-separated values \(CSV\) file\.

## Contents<a name="api-CSVInput-contents"></a>

**Comments**  
A single character used to indicate that a row should be ignored when the character is present at the start of that row\.  
*Type*: String  
*Required*: no

**FieldDelimiter**  
A single character used to separate individual fields from each other within a record\. The character must be a `\n`, `\r`, or an ASCII character in the range 32–126\. The default is a comma \(`,`\)\.  
*Type*: String  
*Default*: ,  
*Required*: no

**FileHeaderInfo**  
A value that describes what to do with the first line of the input\.   
*Type*: String  
*Valid Values*: `Use` \| `Ignore` \| `None`   
*Required*: no

**QuoteCharacter**  
A single character used as an escape character where the field delimiter is part of the value\.  
*Type*: String  
*Required*: no

**QuoteEscapeCharacter**  
A single character used for escaping the quotation\-mark character inside an already escaped value\.  
*Type*: String  
*Required*: no

**RecordDelimiter**  
A single character used to separate individual records from each other\.  
*Type*: String  
*Required*: no

## More Info<a name="more-info-api-CSVInput"></a>
+ [Initiate Job \(POST jobs\)](api-initiate-job-post.md)