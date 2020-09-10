# Configuring Vault Notifications in Amazon S3 Glacier Using the AWS SDK for \.NET<a name="configuring-notifications-sdk-dotnet"></a>

The following are the steps to configure notifications on a vault using the low\-level API of the AWS SDK for \.NET\.

1. Create an instance of the `AmazonGlacierClient` class \(the client\)\. 

   You need to specify an AWS Region where the vault resides\. All operations you perform using this client apply to that AWS Region\. 

1. Provide notification configuration information by creating an instance of the `SetVaultNotificationsRequest` class\.

   You need to provide the vault name, notification configuration information, and account ID\. If you don't provide an account ID, then the account ID associated with the credentials you provide to sign the request is assumed\. For more information, see [Using the AWS SDK for \.NET with Amazon S3 Glacier](using-aws-sdk-for-dot-net.md)\. 

   In specifying a notification configuration, you provide the Amazon Resource Name \(ARN\) of an existing Amazon SNS topic and one or more events for which you want to be notified\. For a list of supported events, see [Set Vault Notification Configuration \(PUT notification\-configuration\)](api-vault-notifications-put.md)\)\.

1. Run the `SetVaultNotifications` method by providing the request object as a parameter\. 

1. After setting notification configuration on a vault, you can retrieve configuration information by calling the `GetVaultNotifications` method, and remove it by calling the `DeleteVaultNotifications` method provided by the client\. 

## Example: Setting the Notification Configuration on a Vault Using the AWS SDK for \.NET<a name="creating-vaults-sdk-dotnet-example-notification"></a>

The following C\# code example illustrates the preceding steps\. The example sets the notification configuration on the vault \("`examplevault`"\) in the US West \(Oregon\) Region, retrieves the configuration, and then deletes it\. The configuration requests Amazon S3 Glacier \(S3 Glacier\) to send a notification to the specified Amazon SNS topic when either the `ArchiveRetrievalCompleted` event or the `InventoryRetrievalCompleted` event occurs\.

**Note**  
For information about the underlying REST API, see [Vault Operations](vault-operations.md)\.

For step\-by\-step instructions to run the following example, see [Running Code Examples](using-aws-sdk-for-dot-net.md#setting-up-and-testing-sdk-dotnet)\. You need to update the code as shown and provide an existing vault name and an Amazon SNS topic\. 

**Example**  

```
using System;
using System.Collections.Generic;
using Amazon.Glacier;
using Amazon.Glacier.Model;
using Amazon.Runtime;

namespace glacier.amazon.com.docsamples
{
  class VaultNotificationSetGetDelete
  {
    static string vaultName   = "examplevault";
    static string snsTopicARN = "*** Provide Amazon SNS topic ARN ***";

    static IAmazonGlacier client;

    public static void Main(string[] args)
    {
      try
      {
        using (client = new AmazonGlacierClient(Amazon.RegionEndpoint.USWest2))
        {
          Console.WriteLine("Adding notification configuration to the vault.");
          SetVaultNotificationConfig();
          GetVaultNotificationConfig();
          Console.WriteLine("To delete vault notification configuration, press Enter");
          Console.ReadKey();
          DeleteVaultNotificationConfig();
        }
      }
      catch (AmazonGlacierException e) { Console.WriteLine(e.Message); }
      catch (AmazonServiceException e) { Console.WriteLine(e.Message); }
      catch (Exception e) { Console.WriteLine(e.Message); }
      Console.WriteLine("To continue, press Enter");
      Console.ReadKey();
    }

    static void SetVaultNotificationConfig()
    {

      SetVaultNotificationsRequest request = new SetVaultNotificationsRequest()
      {     
        VaultName = vaultName,
        VaultNotificationConfig = new VaultNotificationConfig()
        {
          Events   = new List<string>() { "ArchiveRetrievalCompleted", "InventoryRetrievalCompleted" },
          SNSTopic = snsTopicARN
        }
      };
      SetVaultNotificationsResponse response = client.SetVaultNotifications(request);
    }

    static void GetVaultNotificationConfig()
    {
      GetVaultNotificationsRequest request = new GetVaultNotificationsRequest()
      {
        VaultName = vaultName,
        AccountId = "-"
      };
      GetVaultNotificationsResponse response = client.GetVaultNotifications(request);
      Console.WriteLine("SNS Topic ARN: {0}", response.VaultNotificationConfig.SNSTopic);
      foreach (string s in response.VaultNotificationConfig.Events)
        Console.WriteLine("Event : {0}", s);
    }

    static void DeleteVaultNotificationConfig()
    {
      DeleteVaultNotificationsRequest request = new DeleteVaultNotificationsRequest()
      {  
        VaultName = vaultName
      };
      DeleteVaultNotificationsResponse response = client.DeleteVaultNotifications(request);
    }
  }
}
```