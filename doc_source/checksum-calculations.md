# Computing Checksums<a name="checksum-calculations"></a>

When uploading an archive, you must include both the `x-amz-sha256-tree-hash` and `x-amz-content-sha256` headers\. The `x-amz-sha256-tree-hash` header is a checksum of the payload in your request body\. This topic describes how to calculate the `x-amz-sha256-tree-hash` header\. The `x-amz-content-sha256` header is a hash of the entire payload and is required for authorization\. For more information, see [Example Signature Calculation for Streaming API](amazon-glacier-signing-requests.md#example-signature-calculation-streaming)\. 

The payload of your request can be an:
+ **Entire archive—** When uploading an archive in a single request using the Upload Archive API, you send the entire archive in the request body\. In this case, you must include the checksum of the entire archive\. 
+ **Archive part—** When uploading an archive in parts using the multipart upload API, you send only a part of the archive in the request body\. In this case, you include the checksum of the archive part\. And after you upload all the parts, you send a Complete Multipart Upload request, which must include the checksum of the entire archive\.

The checksum of the payload is a SHA\-256 tree hash\. It is called a tree hash because in the process of computing the checksum you compute a tree of SHA\-256 hash values\. The hash value at the root is the checksum for the entire archive\. 

**Note**  
This section describes a way to compute the SHA\-256 tree hash\. However, you may use any procedure as long as it produces the same result\.

You compute the SHA\-256 tree hash as follows:

1. For each 1 MB chunk of payload data, compute the SHA\-256 hash\. The last chunk of data can be less than 1 MB\. For example, if you are uploading a 3\.2 MB archive, you compute the SHA\-256 hash values for each of the first three 1 MB chunks of data, and then compute the SHA\-256 hash of the remaining 0\.2 MB data\. These hash values form the leaf nodes of the tree\.

1. Build the next level of the tree\.

   1. Concatenate two consecutive child node hash values and compute the SHA\-256 hash of the concatenated hash values\. This concatenation and generation of the SHA\-256 hash produces a parent node for the two child nodes\.

   1. When only one child node remains, you promote that hash value to the next level in the tree\.

1. Repeat step 2 until the resulting tree has a root\. The root of the tree provides a hash of the entire archive and a root of the appropriate subtree provides the hash for the part in a multipart upload\. 

**Topics**
+ [Tree Hash Example 1: Uploading an archive in a single request](#checksum-calculations-upload-archive-in-single-payload)
+ [Tree Hash Example 2: Uploading an archive using a multipart upload](#checksum-calculations-upload-archive-using-mpu)
+ [Computing the Tree Hash of a File](#checksum-calculations-examples)
+ [Receiving Checksums When Downloading Data](checksum-calculations-range.md)

## Tree Hash Example 1: Uploading an archive in a single request<a name="checksum-calculations-upload-archive-in-single-payload"></a>

When you upload an archive in a single request using the Upload Archive API \(see [Upload Archive \(POST archive\)](api-archive-post.md)\), the request payload includes the entire archive\. Accordingly, you must include the tree hash of the entire archive in the `x-amz-sha256-tree-hash` request header\. Suppose you want to upload a 6\.5 MB archive\. The following diagram illustrates the process of creating the SHA\-256 hash of the archive\. You read the archive and compute the SHA\-256 hash for each 1 MB chunk\. You also compute the hash for the remaining 0\.5 MB data and then build the tree as outlined in the preceding procedure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/TreeHash-ArchiveUploadSingleRequest.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

## Tree Hash Example 2: Uploading an archive using a multipart upload<a name="checksum-calculations-upload-archive-using-mpu"></a>

The process of computing the tree hash when uploading an archive using multipart upload is the same when uploading the archive in a single request\. The only difference is that in a multipart upload you upload only a part of the archive in each request \(using the [Upload Part \(PUT uploadID\)](api-upload-part.md) API\), and therefore you provide the checksum of only the part in the `x-amz-sha256-tree-hash` request header\. However, after you upload all parts, you must send the Complete Multipart Upload \(see [Complete Multipart Upload \(POST uploadID\)](api-multipart-complete-upload.md)\) request with a tree hash of the entire archive in the `x-amz-sha256-tree-hash` request header\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/images/TreeHash-MPU.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/amazonglacier/latest/dev/)

## Computing the Tree Hash of a File<a name="checksum-calculations-examples"></a>

The algorithms shown here are selected for demonstration purposes\. You can optimize the code as needed for your implementation scenario\. If you are using an AWS SDK to program against Amazon S3 Glacier \(S3 Glacier\), the tree hash calculation is done for you and you only need to provide the file reference\.

**Example 1: Java Example**  
The following example shows how to calculate the SHA256 tree hash of a file using Java\. You can run this example by either supplying a file location as an argument or you can use the `TreeHashExample.computeSHA256TreeHash` method directly from your code\.  

```
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class TreeHashExample {

static final int ONE_MB = 1024 * 1024;

    /**
     * Compute the Hex representation of the SHA-256 tree hash for the specified
     * File
     * 
     * @param args
     *            args[0]: a file to compute a SHA-256 tree hash for
     */
    public static void main(String[] args) {

        if (args.length < 1) {
            System.err.println("Missing required filename argument");
            System.exit(-1);
        }

        File inputFile = new File(args[0]);
        try {

            byte[] treeHash = computeSHA256TreeHash(inputFile);
            System.out.printf("SHA-256 Tree Hash = %s\n", toHex(treeHash));

        } catch (IOException ioe) {
            System.err.format("Exception when reading from file %s: %s", inputFile,
                    ioe.getMessage());
            System.exit(-1);

        } catch (NoSuchAlgorithmException nsae) {
            System.err.format("Cannot locate MessageDigest algorithm for SHA-256: %s",
                    nsae.getMessage());
            System.exit(-1);
        }
    }

    /**
     * Computes the SHA-256 tree hash for the given file
     * 
     * @param inputFile
     *            a File to compute the SHA-256 tree hash for
     * @return a byte[] containing the SHA-256 tree hash
     * @throws IOException
     *             Thrown if there's an issue reading the input file
     * @throws NoSuchAlgorithmException
     */
    public static byte[] computeSHA256TreeHash(File inputFile) throws IOException,
            NoSuchAlgorithmException {

        byte[][] chunkSHA256Hashes = getChunkSHA256Hashes(inputFile);
        return computeSHA256TreeHash(chunkSHA256Hashes);
    }

    /**
     * Computes a SHA256 checksum for each 1 MB chunk of the input file. This
     * includes the checksum for the last chunk even if it is smaller than 1 MB.
     * 
     * @param file
     *            A file to compute checksums on
     * @return a byte[][] containing the checksums of each 1 MB chunk
     * @throws IOException
     *             Thrown if there's an IOException when reading the file
     * @throws NoSuchAlgorithmException
     *             Thrown if SHA-256 MessageDigest can't be found
     */
    public static byte[][] getChunkSHA256Hashes(File file) throws IOException,
            NoSuchAlgorithmException {

        MessageDigest md = MessageDigest.getInstance("SHA-256");

        long numChunks = file.length() / ONE_MB;
        if (file.length() % ONE_MB > 0) {
            numChunks++;
        }

        if (numChunks == 0) {
            return new byte[][] { md.digest() };
        }

        byte[][] chunkSHA256Hashes = new byte[(int) numChunks][];
        FileInputStream fileStream = null;

        try {
            fileStream = new FileInputStream(file);
            byte[] buff = new byte[ONE_MB];

            int bytesRead;
            int idx = 0;

            while ((bytesRead = fileStream.read(buff, 0, ONE_MB)) > 0) {
                md.reset();
                md.update(buff, 0, bytesRead);
                chunkSHA256Hashes[idx++] = md.digest();
            }

            return chunkSHA256Hashes;

        } finally {
            if (fileStream != null) {
                try {
                    fileStream.close();
                } catch (IOException ioe) {
                    System.err.printf("Exception while closing %s.\n %s", file.getName(),
                            ioe.getMessage());
                }
            }
        }
    }

    /**
     * Computes the SHA-256 tree hash for the passed array of 1 MB chunk
     * checksums.
     * 
     * This method uses a pair of arrays to iteratively compute the tree hash
     * level by level. Each iteration takes two adjacent elements from the
     * previous level source array, computes the SHA-256 hash on their
     * concatenated value and places the result in the next level's destination
     * array. At the end of an iteration, the destination array becomes the
     * source array for the next level.
     * 
     * @param chunkSHA256Hashes
     *            An array of SHA-256 checksums
     * @return A byte[] containing the SHA-256 tree hash for the input chunks
     * @throws NoSuchAlgorithmException
     *             Thrown if SHA-256 MessageDigest can't be found
     */
    public static byte[] computeSHA256TreeHash(byte[][] chunkSHA256Hashes)
            throws NoSuchAlgorithmException {

        MessageDigest md = MessageDigest.getInstance("SHA-256");

        byte[][] prevLvlHashes = chunkSHA256Hashes;

        while (prevLvlHashes.length > 1) {

            int len = prevLvlHashes.length / 2;
            if (prevLvlHashes.length % 2 != 0) {
                len++;
            }

            byte[][] currLvlHashes = new byte[len][];

            int j = 0;
            for (int i = 0; i < prevLvlHashes.length; i = i + 2, j++) {

                // If there are at least two elements remaining
                if (prevLvlHashes.length - i > 1) {

                    // Calculate a digest of the concatenated nodes
                    md.reset();
                    md.update(prevLvlHashes[i]);
                    md.update(prevLvlHashes[i + 1]);
                    currLvlHashes[j] = md.digest();

                } else { // Take care of remaining odd chunk
                    currLvlHashes[j] = prevLvlHashes[i];
                }
            }

            prevLvlHashes = currLvlHashes;
        }

        return prevLvlHashes[0];
    }

    /**
     * Returns the hexadecimal representation of the input byte array
     * 
     * @param data
     *            a byte[] to convert to Hex characters
     * @return A String containing Hex characters
     */
    public static String toHex(byte[] data) {
        StringBuilder sb = new StringBuilder(data.length * 2);

        for (int i = 0; i < data.length; i++) {
            String hex = Integer.toHexString(data[i] & 0xFF);

            if (hex.length() == 1) {
                // Append leading zero.
                sb.append("0");
            }
            sb.append(hex);
        }
        return sb.toString().toLowerCase();
    }
}
```

**Example 2: C\# \.NET Example**  
The following example shows how to calculate the SHA256 tree hash of a file\. You can run this example by supplying a file location as an argument\.  

```
using System;
using System.IO;

using System.Security.Cryptography;

namespace ExampleTreeHash
{
    class Program
    {
        static int ONE_MB = 1024 * 1024;

        /**
        * Compute the Hex representation of the SHA-256 tree hash for the
        * specified file
        * 
        * @param args
        *            args[0]: a file to compute a SHA-256 tree hash for
        */
        public static void Main(string[] args)
        {
            if (args.Length < 1)
            {
                Console.WriteLine("Missing required filename argument");
                Environment.Exit(-1);
            }
            FileStream inputFile = File.Open(args[0], FileMode.Open, FileAccess.Read);
            try
            {
                byte[] treeHash = ComputeSHA256TreeHash(inputFile);
                Console.WriteLine("SHA-256 Tree Hash = {0}", BitConverter.ToString(treeHash).Replace("-", "").ToLower());
                Console.ReadLine();
                Environment.Exit(-1);
            }
            catch (IOException ioe)
            {
                Console.WriteLine("Exception when reading from file {0}: {1}",
                    inputFile, ioe.Message);
                Console.ReadLine();
                Environment.Exit(-1);
            }
            catch (Exception e)
            {
                Console.WriteLine("Cannot locate MessageDigest algorithm for SHA-256: {0}",
                    e.Message);
                Console.WriteLine(e.GetType());
                Console.ReadLine();
                Environment.Exit(-1);
            }
            Console.ReadLine();
        }


        /**
         * Computes the SHA-256 tree hash for the given file
         * 
         * @param inputFile
         *            A file to compute the SHA-256 tree hash for
         * @return a byte[] containing the SHA-256 tree hash
         */
        public static byte[] ComputeSHA256TreeHash(FileStream inputFile)
        {
            byte[][] chunkSHA256Hashes = GetChunkSHA256Hashes(inputFile);
            return ComputeSHA256TreeHash(chunkSHA256Hashes);
        }


        /**
         * Computes a SHA256 checksum for each 1 MB chunk of the input file. This
         * includes the checksum for the last chunk even if it is smaller than 1 MB.
         * 
         * @param file
         *            A file to compute checksums on
         * @return a byte[][] containing the checksums of each 1MB chunk
         */
        public static byte[][] GetChunkSHA256Hashes(FileStream file)
        {
            long numChunks = file.Length / ONE_MB;
            if (file.Length % ONE_MB > 0)
            {
                numChunks++;
            }

            if (numChunks == 0)
            {
                return new byte[][] { CalculateSHA256Hash(null, 0) };
            }
            byte[][] chunkSHA256Hashes = new byte[(int)numChunks][];

            try
            {
                byte[] buff = new byte[ONE_MB];

                int bytesRead;
                int idx = 0;

                while ((bytesRead = file.Read(buff, 0, ONE_MB)) > 0)
                {
                    chunkSHA256Hashes[idx++] = CalculateSHA256Hash(buff, bytesRead);
                }
                return chunkSHA256Hashes;
            }
            finally
            {
                if (file != null)
                {
                    try
                    {
                        file.Close();
                    }
                    catch (IOException ioe)
                    {
                        throw ioe;
                    }
                }
            }

        }

        /**
         * Computes the SHA-256 tree hash for the passed array of 1MB chunk
         * checksums.
         * 
         * This method uses a pair of arrays to iteratively compute the tree hash
         * level by level. Each iteration takes two adjacent elements from the
         * previous level source array, computes the SHA-256 hash on their
         * concatenated value and places the result in the next level's destination
         * array. At the end of an iteration, the destination array becomes the
         * source array for the next level.
         * 
         * @param chunkSHA256Hashes
         *            An array of SHA-256 checksums
         * @return A byte[] containing the SHA-256 tree hash for the input chunks
         */
        public static byte[] ComputeSHA256TreeHash(byte[][] chunkSHA256Hashes)
        {
            byte[][] prevLvlHashes = chunkSHA256Hashes;
            while (prevLvlHashes.GetLength(0) > 1)
            {

                int len = prevLvlHashes.GetLength(0) / 2;
                if (prevLvlHashes.GetLength(0) % 2 != 0)
                {
                    len++;
                }

                byte[][] currLvlHashes = new byte[len][];

                int j = 0;
                for (int i = 0; i < prevLvlHashes.GetLength(0); i = i + 2, j++)
                {

                    // If there are at least two elements remaining
                    if (prevLvlHashes.GetLength(0) - i > 1)
                    {

                        // Calculate a digest of the concatenated nodes
                        byte[] firstPart = prevLvlHashes[i];
                        byte[] secondPart = prevLvlHashes[i + 1];
                        byte[] concatenation = new byte[firstPart.Length + secondPart.Length];
                        System.Buffer.BlockCopy(firstPart, 0, concatenation, 0, firstPart.Length);
                        System.Buffer.BlockCopy(secondPart, 0, concatenation, firstPart.Length, secondPart.Length);

                        currLvlHashes[j] = CalculateSHA256Hash(concatenation, concatenation.Length);

                    }
                    else
                    { // Take care of remaining odd chunk
                        currLvlHashes[j] = prevLvlHashes[i];
                    }
                }

                prevLvlHashes = currLvlHashes;
            }

            return prevLvlHashes[0];
        }

        public static byte[] CalculateSHA256Hash(byte[] inputBytes, int count)
        {
            SHA256 sha256 = System.Security.Cryptography.SHA256.Create();
            byte[] hash = sha256.ComputeHash(inputBytes, 0, count);
            return hash;
        }
    }
}
```