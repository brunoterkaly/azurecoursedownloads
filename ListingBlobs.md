## Listing blobs In A container
We also set the permissions of a blob container to public.

```java

package com.terkaly.storage;


import java.io.*;
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

//Two Parts: Part 1 - Upload image to blob storage. 
//Put message into queue with url of blob. 
//Part 2 - reads the message from the queue, downloads 
//the blob, and makes a thumbnail out of it. 
//Upload thumbnail to blob storage.


public class BlobManager {
    public static final String storageConnectionString =
                "DefaultEndpointsProtocol=https;"
                + "AccountName=oreillystorage2;"
                + "AccountKey=[GET THIS FROM THE PORTAL]";
	public static void main(String[] args) {
       
        try {
           CloudStorageAccount account = CloudStorageAccount.parse(storageConnectionString);
           CloudBlobClient serviceClient = account.createCloudBlobClient();
           // Container name must be lower case.
           CloudBlobContainer container = serviceClient.getContainerReference("myimages");
           container.createIfNotExists();
  
           // Upload an image file.
           CloudBlockBlob blob = container.getBlockBlobReference("image.jpg");
           File sourceFile = new File("image.jpg");
           blob.upload(new FileInputStream(sourceFile),  sourceFile.length());
           // Download the image file.
           File destinationFile = new File(sourceFile.getParentFile(), "image1Download.jpg");
           blob.downloadToFile(destinationFile.getAbsolutePath());
           // Make the container public
           // Create a permissions object
           BlobContainerPermissions containerPermissions = new BlobContainerPermissions();
           // Include public access in the permissions object
           containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);;
           // Set the permissions on the container
           container.uploadPermissions(containerPermissions);

           // For each item in the container show it's url
           for (ListBlobItem blobItem : container.listBlobs()) {
               // If the item is a blob, not a virtual directory
               if (blobItem instanceof CloudBlockBlob) {
                // Convert blob (if it is CloudBlockBlob)
                CloudBlockBlob retrievedBlob = (CloudBlockBlob) blobItem;
                java.net.URI uri = retrievedBlob.getUri();
                System.out.println(uri.toString());
               }
           }

        }
        catch (Exception e) {
           System.out.print("Exception encountered: ");
           System.out.println(e.getMessage());
           System.exit(-1);
        }
    }


}
```


