# Uploading Large Archives in Parts Using the AWS SDK for Java<a name="uploading-an-archive-mpu-using-java"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for Java provide a method to upload a large archive \(see [Uploading an Archive in Amazon S3 Glacier](uploading-an-archive.md)\)\. 
+ The high\-level API provides a method that you can use to upload archives of any size\. Depending on the file you are uploading, the method either uploads an archive in a single operation or uses the multipart upload support in Amazon S3 Glacier \(S3 Glacier\) to upload the archive in parts\.
+ The low\-level API maps close to the underlying REST implementation\. Accordingly, it provides a method to upload smaller archives in one operation and a group of methods that support multipart upload for larger archives\. This section explains uploading large archives in parts using the low\-level API\.

For more information about the high\-level and low\-level APIs, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\.

**Topics**
+ [Uploading Large Archives in Parts Using the High\-Level API of the AWS SDK for Java](#uploading-an-archive-in-parts-highlevel-using-java)
+ [Upload Large Archives in Parts Using the Low\-Level API of the AWS SDK for Java](#uploading-an-archive-mpu-using-java-lowlevel)

## Uploading Large Archives in Parts Using the High\-Level API of the AWS SDK for Java<a name="uploading-an-archive-in-parts-highlevel-using-java"></a>

You use the same methods of the high\-level API to upload small or large archives\. Based on the archive size, the high\-level API methods decide whether to upload the archive in a single operation or use the multipart upload API provided by S3 Glacier\. For more information, see [Uploading an Archive Using the High\-Level API of the AWS SDK for Java](uploading-an-archive-single-op-using-java.md#uploading-an-archive-single-op-high-level-using-java)\.

## Upload Large Archives in Parts Using the Low\-Level API of the AWS SDK for Java<a name="uploading-an-archive-mpu-using-java-lowlevel"></a>

For granular control of the upload you can use the low\-level API where you can configure the request and process the response\. The following are the steps to upload large archives in parts using the AWS SDK for Java\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where you want to save the archive\. All operations you perform using this client apply to that AWS Region\. 

1. Initiate multipart upload by calling the `initiateMultipartUpload` method\.

   You need to provide vault name in which you want to upload the archive, part size you want to use to upload archive parts, and an optional description\. You provide this information by creating an instance of the `InitiateMultipartUploadRequest` class\. In response, S3 Glacier returns an upload ID\.

1. Upload parts by calling the `uploadMultipartPart` method\. 

   For each part you upload, You need to provide the vault name, the byte range in the final assembled archive that will be uploaded in this part, the checksum of the part data, and the upload ID\. 

1. Complete multipart upload by calling the `completeMultipartUpload` method\.

   You need to provide the upload ID, the checksum of the entire archive, the archive size \(combined size of all parts you uploaded\), and the vault name\. S3 Glacier constructs the archive from the uploaded parts and returns an archive ID\.

### Example: Uploading a Large Archive in a Parts Using the AWS SDK for Java<a name="upload-archive-mpu-java-example"></a>

The following Java code example uses the AWS SDK for Java to upload an archive to a vault \(`examplevault`\)\. For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the name of the file you want to upload\.

**Note**  
This example is valid for part sizes from 1 MB to 1 GB\. However, S3 Glacier supports part sizes up to 4 GB\. 

**Example**  

```
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.util.Arrays;
import java.util.Date;
import java.util.LinkedList;
import java.util.List;

import com.amazonaws.AmazonClientException;
import com.amazonaws.AmazonServiceException;
import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.TreeHashGenerator;
import com.amazonaws.services.glacier.model.CompleteMultipartUploadRequest;
import com.amazonaws.services.glacier.model.CompleteMultipartUploadResult;
import com.amazonaws.services.glacier.model.InitiateMultipartUploadRequest;
import com.amazonaws.services.glacier.model.InitiateMultipartUploadResult;
import com.amazonaws.services.glacier.model.UploadMultipartPartRequest;
import com.amazonaws.services.glacier.model.UploadMultipartPartResult;
import com.amazonaws.util.BinaryUtils;

public class ArchiveMPU {

    public static String vaultName = "examplevault";
    // This example works for part sizes up to 1 GB.
    public static String partSize = "1048576"; // 1 MB.
    public static String archiveFilePath = "*** provide archive file path ***";
    public static AmazonGlacierClient client;
    
    public static void main(String[] args) throws IOException {

    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier.us-west-2.amazonaws.com/");

        try {
            System.out.println("Uploading an archive.");
            String uploadId = initiateMultipartUpload();
            String checksum = uploadParts(uploadId);
            String archiveId = CompleteMultiPartUpload(uploadId, checksum);
            System.out.println("Completed an archive. ArchiveId: " + archiveId);
            
        } catch (Exception e) {
            System.err.println(e);
        }

    }
    
    private static String initiateMultipartUpload() {
        // Initiate
        InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest()
            .withVaultName(vaultName)
            .withArchiveDescription("my archive " + (new Date()))
            .withPartSize(partSize);            
        
        InitiateMultipartUploadResult result = client.initiateMultipartUpload(request);
        
        System.out.println("ArchiveID: " + result.getUploadId());
        return result.getUploadId();
    }

    private static String uploadParts(String uploadId) throws AmazonServiceException, NoSuchAlgorithmException, AmazonClientException, IOException {

        int filePosition = 0;
        long currentPosition = 0;
        byte[] buffer = new byte[Integer.valueOf(partSize)];
        List<byte[]> binaryChecksums = new LinkedList<byte[]>();
        
        File file = new File(archiveFilePath);
        FileInputStream fileToUpload = new FileInputStream(file);
        String contentRange;
        int read = 0;
        while (currentPosition < file.length())
        {
            read = fileToUpload.read(buffer, filePosition, buffer.length);
            if (read == -1) { break; }
            byte[] bytesRead = Arrays.copyOf(buffer, read);

            contentRange = String.format("bytes %s-%s/*", currentPosition, currentPosition + read - 1);
            String checksum = TreeHashGenerator.calculateTreeHash(new ByteArrayInputStream(bytesRead));
            byte[] binaryChecksum = BinaryUtils.fromHex(checksum);
            binaryChecksums.add(binaryChecksum);
            System.out.println(contentRange);
                        
            //Upload part.
            UploadMultipartPartRequest partRequest = new UploadMultipartPartRequest()
            .withVaultName(vaultName)
            .withBody(new ByteArrayInputStream(bytesRead))
            .withChecksum(checksum)
            .withRange(contentRange)
            .withUploadId(uploadId);               
        
            UploadMultipartPartResult partResult = client.uploadMultipartPart(partRequest);
            System.out.println("Part uploaded, checksum: " + partResult.getChecksum());
            
            currentPosition = currentPosition + read;
        }
        fileToUpload.close();
        String checksum = TreeHashGenerator.calculateTreeHash(binaryChecksums);
        return checksum;
    }

    private static String CompleteMultiPartUpload(String uploadId, String checksum) throws NoSuchAlgorithmException, IOException {
        
        File file = new File(archiveFilePath);

        CompleteMultipartUploadRequest compRequest = new CompleteMultipartUploadRequest()
            .withVaultName(vaultName)
            .withUploadId(uploadId)
            .withChecksum(checksum)
            .withArchiveSize(String.valueOf(file.length()));
        
        CompleteMultipartUploadResult compResult = client.completeMultipartUpload(compRequest);
        return compResult.getLocation();
    }
}
```