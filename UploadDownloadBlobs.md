## Upload and download blobs


```java

package com.terkaly.storage;


import java.io.*;
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.blob.*;

// Upload image to blob storage. 


public class BlobManager {
    public static final String storageConnectionString =
                "DefaultEndpointsProtocol=https;"
                + "AccountName=oreillystorage2;"
                + "AccountKey=[GET THIS FROM THE PORTAL]";
    @SuppressWarnings("unused")
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

           
        }
        catch (Exception e) {
           System.out.print("Exception encountered: ");
           System.out.println(e.getMessage());
           System.exit(-1);
        }
    }


}






```