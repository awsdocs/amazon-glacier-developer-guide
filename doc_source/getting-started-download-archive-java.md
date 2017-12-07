# Download an Archive from a Vault in Amazon Glacier Using the AWS SDK for Java<a name="getting-started-download-archive-java"></a>

The following Java code example uses the high\-level API of the AWS SDK for Java to download the archive you uploaded in the previous step\. In the code example, note the following:

+ The example creates an instance of the `AmazonGlacierClient` class\. 

+ The example uses the `download` method of the `ArchiveTransferManager` class from the high\-level API of the AWS SDK for Java\. 

+ The example uses the US West \(Oregon\) region \(`us-west-2`\) to match the location where you created the vault in [Step 2: Create a Vault in Amazon Glacier](getting-started-create-vault.md)\. 

For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the archive ID of the file you uploaded in [Step 3: Upload an Archive to a Vault in Amazon Glacier](getting-started-upload-archive.md)\. 

**Example — Downloading an Archive Using the AWS SDK for Java**  

```
import java.io.File;
import java.io.IOException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.transfer.ArchiveTransferManager;
import com.amazonaws.services.sns.AmazonSNSClient;
import com.amazonaws.services.sqs.AmazonSQSClient;

public class AmazonGlacierDownloadArchive_GettingStarted {
    public static String vaultName = "examplevault";
    public static String archiveId = "*** provide archive ID ***";
    public static String downloadFilePath  = "*** provide location to download archive ***";
    
    public static AmazonGlacierClient glacierClient;
    public static AmazonSQSClient sqsClient;
    public static AmazonSNSClient snsClient;
    
    public static void main(String[] args) throws IOException {
        
        
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();
    	
        glacierClient = new AmazonGlacierClient(credentials);        
        sqsClient = new AmazonSQSClient(credentials);
        snsClient = new AmazonSNSClient(credentials);
        
        glacierClient.setEndpoint("glacier.us-west-2.amazonaws.com");
        sqsClient.setEndpoint("sqs.us-west-2.amazonaws.com");
        snsClient.setEndpoint("sns.us-west-2.amazonaws.com");

        try {
            ArchiveTransferManager atm = new ArchiveTransferManager(glacierClient, sqsClient, snsClient);
            
            atm.download(vaultName, archiveId, new File(downloadFilePath));
            
        } catch (Exception e)
        {
            System.err.println(e);
        }
    }
}
```