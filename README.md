# LogicAppv2-BlobToSharepoint

This is a Logic App v2, which gets triggered on the back of a file landing in a Blob (using Azure Event grid trigger). 
Looks up in the destination sharepoint folder to see if there is any file with the same name as what landed in Blob.
If file Exists, updates the file
    else create File

As there is no Action in Sharepoint connector which checks if a "File Exists", we are using "List Folder" Action, which will give a array of files in the specified folder. If no Files in the folder - the action returns empty array. 

Filter Array - This action takes an array as input, and outputs only the portions of the array specified in the filter condition. Very useful action when manipulating JSON arrays, and if you want to omit unwanted items in the array. 

The Filter condition used in this action is "Source File Name" = "File Name in the array of files from destination folder". If there is a match, the Filter array outputs that matched item from the array. If no match, empty array is returned.

To check if the Filter Array returned empty array is the condition which we will be checking, to decide wether to create or delete file. 

Based of the output of the condition we will take one of the Create File or Update file path.

Send an email as the last step.