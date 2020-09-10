# Deleting an Archive in Amazon S3 Glacier Using the AWS SDK for Java<a name="deleting-an-archive-using-java"></a>

The following are the steps to delete an archive using the AWS SDK for Java low\-level API\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the archive you want to delete is stored\. All operations you perform using this client apply to that AWS Region\. 

1. Provide request information by creating an instance of the `DeleteArchiveRequest` class\.

   You need to provide an archive ID, a vault name, and your account ID\. If you don't provide an account ID, then account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\.

1. Run the `deleteArchive` method by providing the request object as a parameter\. 

The following Java code snippet illustrates the preceding steps\.

```
AmazonGlacierClient client;

DeleteArchiveRequest request = new DeleteArchiveRequest()
    .withVaultName("*** provide a vault name ***")
    .withArchiveId("*** provide an archive ID ***");

client.deleteArchive(request);
```

**Note**  
For information about the underlying REST API, see [Delete Archive \(DELETE archive\)](api-archive-delete.md)\.

## Example: Deleting an Archive Using the AWS SDK for Java<a name="deleting-an-archive-using-java-example"></a>

The following Java code example uses the AWS SDK for Java to delete an archive\. For step\-by\-step instructions on how to run this example, see [Running Java Examples for Amazon S3 Glacier Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code as shown with a vault name and the archive ID of the archive you want to delete\.

**Example**  

```
import java.io.IOException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.model.DeleteArchiveRequest;

public class ArchiveDelete {

    public static String vaultName = "*** provide vault name ****";
    public static String archiveId = "*** provide archive ID***";
    public static AmazonGlacierClient client;
    
    public static void main(String[] args) throws IOException {
        
    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);
        client.setEndpoint("https://glacier.us-east-1.amazonaws.com/");        

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