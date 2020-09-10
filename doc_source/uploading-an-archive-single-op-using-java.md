# Uploading an Archive in a Single Operation Using the AWS SDK for Java<a name="uploading-an-archive-single-op-using-java"></a>

Both the [high\-level and low\-level APIs](using-aws-sdk.md) provided by the AWS SDK for Java provide a method to upload an archive\.

**Topics**
+ [Uploading an Archive Using the High\-Level API of the AWS SDK for Java](#uploading-an-archive-single-op-high-level-using-java)
+ [Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for Java](#uploading-an-archive-single-op-low-level-using-java)

## Uploading an Archive Using the High\-Level API of the AWS SDK for Java<a name="uploading-an-archive-single-op-high-level-using-java"></a>

The `ArchiveTransferManager` class of the high\-level API provides the `upload` method, which you can use to upload an archive to a vault\.

**Note**  
You can use the `upload` method to upload small or large archives\. Depending on the archive size you are uploading, this method determines whether to upload it in a single operation or use the multipart upload API to upload the archive in parts\.

### Example: Uploading an Archive Using the High\-Level API of the AWS SDK for Java<a name="upload-archive-high-level-java-example"></a>

The following Java code example uploads an archive to a vault \(`examplevault`\) in the US West \(Oregon\) Region \(`us-west-2`\)\. For a list of supported AWS Regions and endpoints, see [Accessing Amazon S3 Glacier](amazon-glacier-accessing.md)\. 

For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the name of the vault you want to upload to and the name of the file you want to upload\.

**Example**  

```
import java.io.File;
import java.io.IOException;
import java.util.Date;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.transfer.ArchiveTransferManager;
import com.amazonaws.services.glacier.transfer.UploadResult;


public class ArchiveUploadHighLevel {
    public static String vaultName = "*** provide vault name ***";
    public static String archiveToUpload = "*** provide name of file to upload ***";
    
    public static AmazonGlacierClient client;
    
    public static void main(String[] args) throws IOException {
        
        
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();
    	
        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier.us-west-2.amazonaws.com/");

        try {
            ArchiveTransferManager atm = new ArchiveTransferManager(client, credentials);
            
            UploadResult result = atm.upload(vaultName, "my archive " + (new Date()), new File(archiveToUpload));
            System.out.println("Archive ID: " + result.getArchiveId());
            
        } catch (Exception e)
        {
            System.err.println(e);
        }
    }
}
```

## Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for Java<a name="uploading-an-archive-single-op-low-level-using-java"></a>

The low\-level API provides methods for all the archive operations\. The following are the steps to upload an archive using the AWS SDK for Java\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where you want to upload the archive\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `UploadArchiveRequest` class\.

   In addition to the data you want to upload, you need to provide a checksum \(SHA\-256 tree hash\) of the payload, the vault name, the content length of the data, and your account ID\. 

   If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\. 

1. Run the `uploadArchive` method by providing the request object as a parameter\. 

   In response, Amazon S3 Glacier \(S3 Glacier\) returns an archive ID of the newly uploaded archive\. 

The following Java code snippet illustrates the preceding steps\. 

```
AmazonGlacierClient client;

UploadArchiveRequest request = new UploadArchiveRequest()
    .withVaultName("*** provide vault name ***")
    .withChecksum(checksum)
    .withBody(new ByteArrayInputStream(body))
    .withContentLength((long)body.length);

UploadArchiveResult uploadArchiveResult = client.uploadArchive(request);

System.out.println("Location (includes ArchiveID): " + uploadArchiveResult.getLocation());
```

### Example: Uploading an Archive in a Single Operation Using the Low\-Level API of the AWS SDK for Java<a name="uploding-single-archive-using-java-example"></a>

The following Java code example uses the AWS SDK for Java to upload an archive to a vault \(`examplevault`\)\. For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the name of the vault you want to upload to and the name of the file you want to upload\.

```
import java.io.ByteArrayInputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.TreeHashGenerator;
import com.amazonaws.services.glacier.model.UploadArchiveRequest;
import com.amazonaws.services.glacier.model.UploadArchiveResult;
public class ArchiveUploadLowLevel {

    public static String vaultName = "*** provide vault name ****";
    public static String archiveFilePath = "*** provide to file upload ****";
    public static AmazonGlacierClient client;
    
    public static void main(String[] args) throws IOException {
    	
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier.us-east-1.amazonaws.com/");

        try {
            // First open file and read.
            File file = new File(archiveFilePath);
            InputStream is = new FileInputStream(file); 
            byte[] body = new byte[(int) file.length()];
            is.read(body);
                                    
            // Send request.
            UploadArchiveRequest request = new UploadArchiveRequest()
                .withVaultName(vaultName)
                .withChecksum(TreeHashGenerator.calculateTreeHash(new File(archiveFilePath))) 
                .withBody(new ByteArrayInputStream(body))
                .withContentLength((long)body.length);
            
            UploadArchiveResult uploadArchiveResult = client.uploadArchive(request);
            
            System.out.println("ArchiveID: " + uploadArchiveResult.getArchiveId());
            
        } catch (Exception e)
        {
            System.err.println("Archive not uploaded.");
            System.err.println(e);
        }
    }
}
```