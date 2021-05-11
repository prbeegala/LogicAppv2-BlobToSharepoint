# 1. LogicAppv2-BlobToSharepoint

This is a single tenant Logic App -  which gets triggered on the back of a file landing in a Blob (using Azure Event grid trigger). One of the common scenarios to trigger a logic app.
Once it receives the File from blob, the logic app attempts to create this file in Sharepoint. It also ensures that the file is updated, if Sharepoint has a file with the same name.

As there is no Action in Sharepoint connector which checks if a "File Exists", we are using "**List Folder**" Action, which will give a array of files in the specified folder. If no Files in the folder - the action returns empty array. This is what we will check in the If condition later, emptiness of the array, to decide to create or update a file.

**Filter Array** - This action takes an array as input, and outputs only the portions of the array specified in the filter condition. Very useful action when manipulating JSON arrays, and if you want to omit unwanted items in the array. 

The Filter condition used in this action is "Source File Name" = "File Name in the array of files from destination folder". If there is a match, the Filter array outputs that matched item from the array. If no match, empty array is returned.

To check if the Filter Array returned empty array is the condition which we will be checking, to decide wether to create or delete file. 

Based of the output of the condition we will take one of the Create File or Update file path.

Send an email as the last step.

# 2. Appendix

Please note that I'm using the information which is coming from the Event grid trigger message to get hte Blob content. Specifically using the "subject" element, splitting it up to get the correct path which is the container name and the file name. Using Split on function to get the desired values. 

This is how the event message which triggers the logic app looks like. 

```
{
  "topic": "/subscriptions/XCCCCCCCC-6f09-XXXX-8707-4dc848cba302/resourceGroups/rg-pb/providers/Microsoft.Storage/storageAccounts/sample",
  "subject": "/blobServices/default/containers/tescotesting/blobs/Order API Swagger.json",
  "eventType": "Microsoft.Storage.BlobCreated",
  "id": "b76c33f2-001e-0095-3ab0-46405a0632d8",
  "data": {
    "api": "PutBlob",
    "clientRequestId": "cb3c6924-eeab-4045-bfd9-853fed62d0fa",
    "requestId": "b76c33f2-001e-0095-3ab0-46405a000000",
    "eTag": "0x8D914C73A8E2574",
    "contentType": "application/json",
    "contentLength": 5103,
    "blobType": "BlockBlob",
    "url": "https://pbstoraXXXXXXXX.blob.core.windows.net/tescotesting/Order API Swagger.json",
    "sequencer": "00000000000000000000000000001E11000000000007a8f1",
    "storageDiagnostics": {
      "batchId": "926fc2ba-8006-008b-00b0-46ac82000000"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1",
  "eventTime": "2021-05-11T21:53:36.3087505Z"
}
```
![Logic App](/images/LogicAppScreenShot.png)