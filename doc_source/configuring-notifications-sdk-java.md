# Configuring Vault Notifications in Amazon S3 Glacier Using the AWS SDK for Java<a name="configuring-notifications-sdk-java"></a>

The following are the steps to configure notifications on a vault using the low\-level API of the AWS SDK for Java\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the vault resides\. All operations you perform using this client apply to that AWS Region\. 

1. Provide notification configuration information by creating an instance of the `SetVaultNotificationsRequest` class\.

   You need to provide the vault name, notification configuration information, and account ID\. In specifying a notification configuration, you provide the Amazon Resource Name \(ARN\) of an existing Amazon SNS topic and one or more events for which you want to be notified\. For a list of supported events, see [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)\)\.

1. Run the `setVaultNotifications` method by providing the request object as a parameter\. 

The following Java code snippet illustrates the preceding steps\. The snippet sets a notification configuration on a vault\. The configuration requests Amazon S3 Glacier \(S3 Glacier\) to send a notification to the specified Amazon SNS topic when either the `ArchiveRetrievalCompleted` event or the `InventoryRetrievalCompleted` event occurs\.

```
SetVaultNotificationsRequest request = new SetVaultNotificationsRequest()
        .withAccountId("-")
        .withVaultName("*** provide vault name ***")
        .withVaultNotificationConfig(
                new VaultNotificationConfig()
                .withSNSTopic("*** provide SNS topic ARN ***")
                .withEvents("ArchiveRetrievalCompleted", "InventoryRetrievalCompleted")
         );
client.setVaultNotifications(request);
```

**Note**  
For information about the underlying REST API, see [Vault Operations](vault-operations.md)\.

## Example: Setting the Notification Configuration on a Vault Using the AWS SDK for Java<a name="configuring-notifications-sdk-java-example"></a>

The following Java code example sets a vault's notifications configuration, deletes the configuration, and then restores the configuration\. For step\-by\-step instructions on how to run the following example, see [Using the AWS SDK for Java with Amazon S3 Glacier](using-aws-sdk-for-java.md)\. 

**Example**  

```
import java.io.IOException;

import com.amazonaws.auth.profile.ProfileCredentialsProvider;
import com.amazonaws.services.glacier.AmazonGlacierClient;
import com.amazonaws.services.glacier.model.DeleteVaultNotificationsRequest;
import com.amazonaws.services.glacier.model.GetVaultNotificationsRequest;
import com.amazonaws.services.glacier.model.GetVaultNotificationsResult;
import com.amazonaws.services.glacier.model.SetVaultNotificationsRequest;
import com.amazonaws.services.glacier.model.VaultNotificationConfig;


public class AmazonGlacierVaultNotifications {

    public static AmazonGlacierClient client;
    public static String vaultName = "*** provide vault name ****";
    public static String snsTopicARN = "*** provide sns topic ARN ***";

    public static void main(String[] args) throws IOException {

    	ProfileCredentialsProvider credentials = new ProfileCredentialsProvider();

        client = new AmazonGlacierClient(credentials);        
        client.setEndpoint("https://glacier.us-east-1.amazonaws.com/");

        try {

            System.out.println("Adding notification configuration to the vault.");
            setVaultNotifications();
            getVaultNotifications();
            deleteVaultNotifications();
            
        } catch (Exception e) {
            System.err.println("Vault operations failed." + e.getMessage());
        }
    }

    private static void setVaultNotifications() {
        VaultNotificationConfig config = new VaultNotificationConfig()
            .withSNSTopic(snsTopicARN)
            .withEvents("ArchiveRetrievalCompleted", "InventoryRetrievalCompleted");
        
        SetVaultNotificationsRequest request = new SetVaultNotificationsRequest()
                .withVaultName(vaultName)
                .withVaultNotificationConfig(config);
                                
        client.setVaultNotifications(request);
        System.out.println("Notification configured for vault: " + vaultName);
    }

    private static void getVaultNotifications() {
        VaultNotificationConfig notificationConfig = null;
        GetVaultNotificationsRequest request = new GetVaultNotificationsRequest()
                .withVaultName(vaultName);
        GetVaultNotificationsResult result = client.getVaultNotifications(request);
        notificationConfig = result.getVaultNotificationConfig();

        System.out.println("Notifications configuration for vault: "
                + vaultName);
        System.out.println("Topic: " + notificationConfig.getSNSTopic());
        System.out.println("Events: " + notificationConfig.getEvents());
    }

    private static void deleteVaultNotifications() {
            DeleteVaultNotificationsRequest request = new DeleteVaultNotificationsRequest()
                .withVaultName(vaultName);
            client.deleteVaultNotifications(request);
            System.out.println("Notifications configuration deleted for vault: " + vaultName);
    }
}
```