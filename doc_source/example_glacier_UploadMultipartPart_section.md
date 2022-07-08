# Create a multipart upload to an Amazon S3 Glacier vault using an AWS SDK<a name="example_glacier_UploadMultipartPart_section"></a>

The following code example shows how to create a multipart upload to an Amazon S3 Glacier vault\.

**Note**  
The source code for these examples is in the [AWS Code Examples GitHub repository](https://github.com/awsdocs/aws-doc-sdk-examples)\. Have feedback on a code example? [Create an Issue](https://github.com/awsdocs/aws-doc-sdk-examples/issues/new/choose) in the code examples repo\. 

------
#### [ JavaScript ]

**SDK for JavaScript V2**  
 To learn how to set up and run this example, see [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/glacier#code-examples)\. 
Create a multipart upload of 1 megabyte chunks of a Buffer object\.  

```
// Create a new service object and some supporting variables
var glacier = new AWS.Glacier({apiVersion: '2012-06-01'}),
    vaultName = 'YOUR_VAULT_NAME',
    buffer = new Buffer(2.5 * 1024 * 1024), // 2.5MB buffer
    partSize = 1024 * 1024, // 1MB chunks,
    numPartsLeft = Math.ceil(buffer.length / partSize),
    startTime = new Date(),
    params = {vaultName: vaultName, partSize: partSize.toString()};

// Compute the complete SHA-256 tree hash so we can pass it
// to completeMultipartUpload request at the end
var treeHash = glacier.computeChecksums(buffer).treeHash;

// Initiate the multipart upload
console.log('Initiating upload to', vaultName);
// Call Glacier to initiate the upload.
glacier.initiateMultipartUpload(params, function (mpErr, multipart) {
    if (mpErr) { console.log('Error!', mpErr.stack); return; }
    console.log("Got upload ID", multipart.uploadId);

    // Grab each partSize chunk and upload it as a part
    for (var i = 0; i < buffer.length; i += partSize) {
        var end = Math.min(i + partSize, buffer.length),
            partParams = {
                vaultName: vaultName,
                uploadId: multipart.uploadId,
                range: 'bytes ' + i + '-' + (end-1) + '/*',
                body: buffer.slice(i, end)
            };

        // Send a single part
        console.log('Uploading part', i, '=', partParams.range);
        glacier.uploadMultipartPart(partParams, function(multiErr, mData) {
            if (multiErr) return;
            console.log("Completed part", this.request.params.range);
            if (--numPartsLeft > 0) return; // complete only when all parts uploaded

            var doneParams = {
                vaultName: vaultName,
                uploadId: multipart.uploadId,
                archiveSize: buffer.length.toString(),
                checksum: treeHash // the computed tree hash
            };

            console.log("Completing upload...");
            glacier.completeMultipartUpload(doneParams, function(err, data) {
                if (err) {
                    console.log("An error occurred while uploading the archive");
                    console.log(err);
                } else {
                    var delta = (new Date() - startTime) / 1000;
                    console.log('Completed upload in', delta, 'seconds');
                    console.log('Archive ID:', data.archiveId);
                    console.log('Checksum:  ', data.checksum);
                }
            });
        });
    }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/glacier-example-multipart-upload.html)\. 
+  For API details, see [UploadMultipartPart](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/glacier-2012-06-01/UploadMultipartPart) in *AWS SDK for JavaScript API Reference*\. 

------

For a complete list of AWS SDK developer guides and code examples, see [Using S3 Glacier with an AWS SDK](sdk-general-information-section.md)\. This topic also includes information about getting started and details about previous SDK versions\.