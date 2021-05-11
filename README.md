# LogicAppv2-BlobToSharepoint

This is a Logic App v2, which gets triggered on the back of a file landing in a Blob (using Azure Event grid trigger). 
Looks up in the destination sharepoint folder to see if there is any file with the same name as what landed in Blob.
If file Exists, updates the file
    else create File