# Key Management<a name="key-management"></a>

Server\-side encryption addresses data encryption at rest—that is, Amazon S3 Glacier encrypts your data as it writes it to its data centers and decrypts it for you when you access it\. As long as you authenticate your request and you have access permissions, there is no difference in the way you access encrypted or unencrypted data\. 

Data at rest stored in S3 Glacier is automatically server\-side encrypted using AES\-256, using keys maintained by AWS\. As an additional safeguard, AWS encrypts the key itself with a master key that we regularly rotate\. 