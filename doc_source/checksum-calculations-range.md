# Receiving Checksums When Downloading Data<a name="checksum-calculations-range"></a>

When you retrieve an archive using the Initiate Job API \(see [Initiate Job \(POST jobs\)](api-initiate-job-post.md)\), you can optionally specify a range to retrieve of the archive\. Similarly, when you download your data using the Get Job Output API \(see [Get Job Output \(GET output\)](api-job-output-get.md)\), you can optionally specify a range of data to download\. There are two characteristics of these ranges that are important to understand when you are retrieving and downloading your archive's data\. The range to retrieve is required to be *megabyte aligned* to the archive\. Both the range to retrieve and the range to download must be *tree hash aligned* in order to receive checksum values when you download your data\. The definition of these two types of range alignments are as follows:
+ Megabyte aligned \- A range \[*StartByte*, *EndBytes*\] is megabyte \(1024\*1024\) aligned when *StartBytes* is divisible by 1 MB and *EndBytes* plus 1 is divisible by 1 MB or is equal to the end of the archive specified \(archive byte size minus 1\)\. A range used in the Initiate Job API, if specified, is required to be megabyte aligned\.
+ Tree\-hash aligned \- A range \[*StartBytes*, *EndBytes*\] is tree hash aligned with respect to an archive if and only if the root of the tree hash built over the range is equivalent to a node in the tree hash of the whole archive\. Both the range to retrieve and range to download must be tree hash aligned in order to receive checksum values for the data you download\. For an example of ranges and their relationship to the archive tree hash, see [Tree Hash Example: Retrieving an archive range that is tree\-hash aligned](#checksum-calculations-upload-archive-with-ranges)\. 

  Note that a range that is tree\-hash aligned is also megabyte aligned\. However, a megabyte aligned range is not necessarily tree\-hash aligned\. 

The following cases describe when you receive a checksum value when you download your archive data:
+ If you do not specify a range to retrieve in the Initiate Job request and you download the whole archive in the Get Job Request\. 
+ If you do not specify a range to retrieve in the Initiate Job request and you do specify a tree\-hash aligned range to download in the Get Job Request\.
+ If you specify a tree\-hash aligned range to retrieve in the Initiate Job request and you download the whole range in the Get Job Request\. 
+ If you specify a tree\-hash aligned range to retrieve in the Initiate Job request and you specify a tree\-hash aligned range to download in the Get Job Request\. 

If you specify a range to retrieve in the Initiate Job request that is not tree hash aligned, then you can still get your archive data but no checksum values are returned when you download data in the Get Job Request\.

## Tree Hash Example: Retrieving an archive range that is tree\-hash aligned<a name="checksum-calculations-upload-archive-with-ranges"></a>

Suppose you have a 6\.5 MB archive in your vault and you want to retrieve 2 MB of the archive\. How you specify the 2 MB range in the Initiate Job request determines if you receive data checksum values when you download your data\. The following diagram illustrates two 2 MB ranges for the 6\.5 MB archive that you could download\. Both ranges are megabyte aligned, but only one is tree\-hash aligned\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/TreeHash-ArchiveWithRanges.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

## Tree\-Hash Aligned Range Specification<a name="tree-hash-algorithm"></a>

This section gives the exact specification for what constitutes a tree\-hash aligned range\. Tree\-hash aligned ranges are important when you are downloading a portion of an archive and you specify the range of data to retrieve and the range to download from the retrieved data\. If both of these ranges are tree\-hash aligned, then you will receive checksum data when you download the data\. 

A range \[*A*, *B*\] is *tree\-hash aligned* with respect to an archive if and only if when a new tree hash is built over \[*A*, *B*\], the root of the tree hash of that range is equivalent to a node in the tree hash of the whole archive\. You can see this shown in the diagram in [Tree Hash Example: Retrieving an archive range that is tree\-hash aligned](#checksum-calculations-upload-archive-with-ranges)\. In this section, we provide the specification for tree\-hash alignment\.

Consider \[*P*, *Q*\) as the range query for an archive of *N* megabytes \(MB\) and *P* and *Q* are multiples of one MB\. Note that the actual inclusive range is \[*P* MB, *Q* MB – 1 byte\], but for simplicity, we show it as \[*P*, *Q*\)\. With these considerations, then
+ If *P* is an odd number, there is only one possible tree\-hash aligned range—that is \[*P*, *P* \+ 1 MB\)\.
+ If *P* is an even number and *k* is the maximum number, where *P* can be written as 2*k* \* *X*, then there are at most *k* tree\-hash aligned ranges that start with *P*\. *X* is an integer greater than 0\. The tree\-hash aligned ranges fall in the following categories: 
  + For each *i*, where \(0 <= *i* <= *k*\) and where *P* \+ 2*i* < *N*, then \[*P*, *Q* \+ 2*i*\) is a tree\-hash aligned range\.
  + *P* = 0 is the special case where *A* = 2\[lgN\]\*0