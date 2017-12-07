# Amazon Glacier Select SQL Reference<a name="glacier-select-sql-reference"></a>

This section describes the SQL reference for Amazon Glacier select\.


+ [SELECT Command](#glacier-select-sql-reference-select)
+ [Supported Data Types](#glacier-select-sql-reference-data-types)
+ [Data Type Conversions](#glacier-select-sql-reference-data-conversion)
+ [Operators](#glacier-select-sql-reference-operators)
+ [SQL Functions](#glacier-select-sql-reference-sql-functions)

## SELECT Command<a name="glacier-select-sql-reference-select"></a>

Amazon Glacier select supports only the `SELECT` SQL command\. The following ANSI standard clauses are supported for `SELECT`: 

+ `SELECT` list

+ `FROM` clause 

+ `WHERE` clause

**Note**  
Amazon Glacier select queries currently do not support subqueries or joins\.

### FROM Clause<a name="glacier-select-sql-reference-from"></a>

Amazon Glacier select supports the following forms of the `FROM` clause:

```
FROM table_name
FROM table_name alias
FROM table_name AS alias
```

Where `table_name` is one of `ARCHIVE` or `OBJECT` referring to the archive being queried over\. For users coming from traditional relational databases, this can be thought of as a database schema containing two views \(named `ARCHIVE` and `OBJECT` respectively\) over a single table\.

Following standard SQL, the `FROM` clause creates rows that are filtered in the `WHERE` clause and projected in the `SELECT` list\. 

### Attribute Access in the SELECT and WHERE Clauses<a name="glacier-select-sql-reference-Variables"></a>

The `SELECT` and `WHERE` clauses can refer to the N\-th column of a row with the column name \_N, where N is the column position\. The position count starts at 1\. For example, the first column is named \_1 and the second column is named \_2\.

A column can be referred to as \_N or alias\.\_N\. For example, \_2 and myAlias\.\_2 are both valid ways to refer to a column in the `SELECT` list and `WHERE` clause\.

### Scalar Expressions<a name="glacier-select-sql-reference-scalar"></a>

Within the `WHERE` clause and the `SELECT` list, you can have SQL *scalar expressions*, which are expressions that return scalar values\. They have the following form:

+ ***literal*** 

  An SQL literal\. 

+ ***column\_reference*** 

  A reference to a column in the form *column\_name* or *alias\.column\_name*\. 

+ ***unary\_op*** ***expression*** 

  Where ***unary\_op*** unary is an SQL unary operator\.

+ ***expression*** ***binary\_op*** ***expression*** 

   Where ****binary\_op**** is an SQL binary operator\. 

+ **func\_name** 

   Where **func\_name** is the name of a scalar function to invoke\. 

+ ***expression*** `[ NOT ] BETWEEN` ***expression*** `AND` ***expression***

+ ***expression*** `LIKE` ***expression*** \[ `ESCAPE` ***expression*** \]

### WHERE Clause<a name="glacier-select-sql-reference-where"></a>

The `WHERE` clause follows this syntax: 

```
WHERE condition
```

The `WHERE` clause filters rows based on the *condition*\. A *condition* is an expression that has a Boolean result\. Only rows for which the *condition* evaluates to `TRUE` are returned in the result\.

### SELECT List<a name="glacier-select-sql-reference-select-list"></a>

The `SELECT` list names the columns, functions, and expressions that you want the query to return\. The list represents the output of the query\. 

```
SELECT *
SELECT projection [ AS column_alias | column_alias ] [, ...]
```

The first form with \* \(asterisk\) returns every row that passed the `WHERE` clause, as\-is\. The second form creates a row with user\-defined output scalar expressions ***projection*** for each column\.

## Supported Data Types<a name="glacier-select-sql-reference-data-types"></a>


**Amazon Glacier select supports the following subset of primitive data types\.**  

|  Name  |  Description  |  Required  | 
| --- | --- | --- | 
| bool | TRUE or FALSE | FALSE | 
| int | 8\-byte signed integer in the range \-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807\.  | 100000 | 
| string | UTF8\-encoded variable\-length string\. The default limit is 1 character\. The maximum character limit is 2,147,483,647\.  | 'xyz' | 
| float | 8\-byte floating point number\.  | CAST\(0\.456 AS FLOAT\) | 
| decimal | 38\-digit precision number, precision is p, and scale is s\. | 123\.456  | 
| timestamp |  Time stamps represent a specific moment in time, always include a local offset, and are capable of arbitrary precision\. In the text format, time stamps follow the [W3C note on date and time formats](https://www.w3.org/TR/NOTE-datetime), but they must end with the literal “T” if not at least whole\-day precision\. Fractional seconds are allowed, with at least one digit of precision, and an unlimited maximum\. Local\-time offsets may be represented as either hour:minute offsets from UTC, or as the literal “Z” to denote a local time of UTC\. They are required on time stamps with time and are not allowed on date values\.  | CAST\('2007\-04\-05T14:30Z' AS TIMESTAMP\) | 

## Data Type Conversions<a name="glacier-select-sql-reference-data-conversion"></a>

The general rule is to follow the `CAST` function if defined\. If it is not defined, then follow the conversion rules in the next section\. For more information about `CAST`, see [SQL Functions](#glacier-select-sql-reference-sql-functions)\.

**Data Type Conversion Rules**

The following are the data type conversion rules: All input data is treated as a string\. It must be cast into the relevant data types when necessary\.

## Operators<a name="glacier-select-sql-reference-operators"></a>

The following operators are supported\.

### Logical Operators<a name="glacier-select-sql-reference-loical-ops"></a>

+ `AND`

+ `NOT`

+ `OR`

### Comparison Operators<a name="glacier-select-sql-reference-compare-ops"></a>

+ `<` 

+ `>` 

+ `<=`

+ `>=`

+ `=`

+ `<>`

+ `IN` – Amazon Glacier select supports only `IN` for a fixed set of scalar values, for example: `IN ('a', 'b', 'c')`

+ `BETWEEN`

### Pattern Matching Operators<a name="glacier-select-sql-reference-pattern"></a>

+ `LIKE`

### Math Operators<a name="glacier-select-sql-referencemath-ops"></a>

Addition, subtraction, multiplication, division, and modulo are supported\.

+ \+

+ \-

+ \*

+ %

### Operator Precedence<a name="glacier-select-sql-reference-op-Precedence"></a>


**The following table shows operators precedence or in decreasing order\.**  

|  Operator/Element  |  Associativity |  Required  | 
| --- | --- | --- | 
| \-  | right  | unary minus  | 
| \*, /, %  | left  | multiplication, division, modulo  | 
| \+, \-  | left  | addition, subtraction  | 
| IN |  | set membership  | 
| BETWEEN |  | range containment  | 
| LIKE |  | string pattern matching  | 
| <> |  | less than, greater than  | 
| = | right  | equality, assignment | 
| NOT | right | logical negation  | 
| AND | left | logical conjunction  | 
| OR | left | logical disjunction  | 

## SQL Functions<a name="glacier-select-sql-reference-sql-functions"></a>

Data Type Conversion Function 

+ `CAST `

The `CAST` function converts an entity, such as an expression that evaluates to a single value, from one type to another\. 

```
CAST (<expression> AS <data type>)
```

*expression *

A combination of one or more values, operators, and SQL functions that evaluate to a value\. 

*data type* 

The target data type, such as `INT`, to cast the expression to\. 