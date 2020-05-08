# Upload an Archive to a Vault in Amazon S3 Glacier Using the AWS SDK for Java<a name="getting-started-upload-archive-java"></a>

The following Java code example uses the high\-level API of the AWS SDK for Java to upload a sample archive to the vault\. In the code example, note the following:
+ The example creates an instance of the `AmazonGlacierClient` class\. 
+ The example uses the `upload` method of the `ArchiveTransferManager` class from the high\-level API of the AWS SDK for Java\. 
+ The example uses the US West \(Oregon\) Region \(`us-west-2`\) to match the location where you created the vault previously in [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\.

For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the name of the archive file you want to upload\.

**Note**  
Amazon S3 Glacier \(S3 Glacier\) keeps an inventory of all the archives in your vaults\. When you upload the archive in the following example, it will not appear in a vault in the management console until the vault inventory has been updated\. This update usually happens once a day\. 

**Example — Uploading an Archive Using the AWS SDK for Java**  <a name="GS_ExampleUploadArchiveJava"></a>

```
import java.io.File;
import java.io.IOException;
import java.util.Date;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.transfer.ArchiveTransferManager;
import com.amazonaws.services.glacier.transfer.UploadResult;

public class AmazonGlacierUploadArchive_GettingStarted {

    public static String vaultName = "examplevault2";
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