# Delete an Archive from a Vault in Amazon S3 Glacier Using the AWS SDK for Java<a name="getting-started-delete-archive-java"></a>

The following code example uses the AWS SDK for Java to delete the archive\. In the code, note the following:
+ The `DeleteArchiveRequest` object describes the delete request, including the vault name where the archive is located and the archive ID\.
+ The `deleteArchive` method sends the request to Amazon S3 Glacier \(S3 Glacier\) to delete the archive\. 
+ The example uses the US West \(Oregon\) Region \(`us-west-2`\) to match the location where you created the vault in [Step 2: Create a Vault in Amazon S3 Glacier](getting-started-create-vault.md)\. 

For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with the archive ID of the file you uploaded in [Step 3: Upload an Archive to a Vault in Amazon S3 Glacier](getting-started-upload-archive.md)\. 

**Example — Deleting an Archive Using the AWS SDK for Java**  <a name="GS_ExampleDeleteArchiveJava"></a>

```
import java.io.IOException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.model.DeleteArchiveRequest;

public class AmazonGlacierDeleteArchive_GettingStarted {

    public static String vaultName = "examplevault";
    public static String archiveId = "*** provide archive ID***";
    public static AmazonGlacierClient client;
    
    public static void main(String[] args) throws IOException {
        
    	
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier.us-west-2.amazonaws.com/");        

        try {

            // Delete the archive.
            client.deleteArchive(new DeleteArchiveRequest()
                .withVaultName(vaultName)
                .withArchiveId(archiveId));
            
            System.out.println("Deleted archive successfully.");
            
        } catch (Exception e) {
            System.err.println("Archive not deleted.");
            System.err.println(e);
        }
    }
}
```