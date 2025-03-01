---
title: Create account SAS tokens - JavaScript
titleSuffix: Azure Storage
description: Create and use account SAS tokens in a JavaScript application that works with Azure Blob Storage. This article helps you set up a project and authorizes access to an Azure Blob Storage endpoint.
services: storage
author: pauljewellmsft
ms.author: pauljewell

ms.service: storage
ms.topic: how-to
ms.date: 07/05/2022

ms.subservice: blobs
ms.custom: template-how-to, devx-track-js
---

# Create and use account SAS tokens with Azure Blob Storage and JavaScript

This article shows you how to create and use account SAS tokens to use the Azure Blob Storage client library v12 for JavaScript. Once connected, your code can operate on containers, blobs, and features of the Blob Storage service.

The [sample code snippets](https://github.com/Azure-Samples/AzureStorageSnippets/tree/master/blobs/howto/JavaScript/NodeJS-v12/dev-guide) are available in GitHub as runnable Node.js files.

[Package (npm)](https://www.npmjs.com/package/@azure/storage-blob) | [Samples](../common/storage-samples-javascript.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-samples) | [API reference](/javascript/api/preview-docs/@azure/storage-blob) | [Library source code](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/storage/storage-blob) | [Give Feedback](https://github.com/Azure/azure-sdk-for-js/issues)

## Account SAS tokens

An **account SAS token** is one [type of SAS token](../common/storage-sas-overview.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#types-of-shared-access-signatures) for access delegation provided by Azure Storage. An account SAS token provides access to Azure Storage. The token is only as restrictive as you define it when creating it. Because anyone with the token can use it to access your Storage account, you should define the token with the most restrictive permissions that still allow the token to complete the required tasks.

[Best practices for token](../common/storage-sas-overview.md#best-practices-when-using-sas) creation include limiting permissions:

* Services: blob, file, queue, table
* Resource types: service, container, or object
* Permissions such as create, read, write, update, and delete

## Add required dependencies to your application

Include the required dependencies to create an account SAS token.

:::code language="javascript" source="~/azure_storage-snippets/blobs/howto/JavaScript/NodeJS-v12/dev-guide/create-account-sas.js" id="Snippet_Dependencies":::

## Get environment variables to create shared key credential

Use the Blob Storage account name and key to create a [StorageSharedKeyCredential](/javascript/api/@azure/storage-blob/storagesharedkeycredential). This key is required to create the SAS token and to use the SAS token.

Create a [StorageSharedKeyCredential](/javascript/api/@azure/storage-blob/storagesharedkeycredential) by using the storage account name and account key. Then use the StorageSharedKeyCredential to initialize a [BlobServiceClient](/javascript/api/@azure/storage-blob/blobserviceclient).

:::code language="javascript" source="~/azure_storage-snippets/blobs/howto/JavaScript/NodeJS-v12/dev-guide/create-account-sas.js" id="Snippet_EnvironmentVariables":::

## Async operation boilerplate

The remaining sample code snippets assume the following async boilerplate code for Node.js. 

:::code language="javascript" source="~/azure_storage-snippets/blobs/howto/JavaScript/NodeJS-v12/dev-guide/create-account-sas.js" id="Snippet_AsyncBoilerplate":::

## Create SAS token

Because this token can be used with blobs, queues, tables, and files, some of the settings are more broad than just blob options.

1. Create the options object. 
    
    The scope of the abilities of a SAS token is defined by the [AccountSASSignatureValues](/javascript/api/@azure/storage-blob/accountsassignaturevalues). 

    Use the following helper functions provided by the SDK to create the correct value types for the values:

    * [AccountSASServices.parse("btqf").toString()](/javascript/api/@azure/storage-blob/accountsasservices):
        * b: blob
        * t: table
        * q: query
        * f: file
    * [resourceTypes: AccountSASResourceTypes.parse("sco").toString()](/javascript/api/@azure/storage-blob/accountsasresourcetypes)
        * s: service
        * c: container - such as blob container, table or queue
        * o: object - blob, row, message
    * [permissions: AccountSASPermissions.parse("rwdlacupi")](/javascript/api/@azure/storage-blob/accountsaspermissions)
        * r: read
        * w: write
        * d: delete
        * l: list
        * f: filter
        * a: add
        * c: create
        * u: update
        * t: tag access
        * p: process - such as process messages in a queue 
        * i: set immutability policy

1. Pass the object to the [generateAccountSASQueryParameters](/javascript/api/@azure/storage-blob/#@azure-storage-blob-generateaccountsasqueryparameters) function, along with the [SharedKeyCredential](/javascript/api/@azure/storage-blob/#@azure-storage-blob-generateaccountsasqueryparameters), to create the SAS token. 

    Before returning the SAS token, prepend the query string delimiter, `?`. 

    :::code language="javascript" source="~/azure_storage-snippets/blobs/howto/JavaScript/NodeJS-v12/dev-guide/create-account-sas.js" id="Snippet_GetSas":::

1. Secure the SAS token until it is used. 

## Use Blob service with account SAS token

To use the account SAS token, you need to combine it with the account name to create the URI. Pass the URI to create the blobServiceClient. Once you have the blobServiceClient, you can use that client to access your Blob service. 
 
:::code language="javascript" source="~/azure_storage-snippets/blobs/howto/JavaScript/NodeJS-v12/dev-guide/create-account-sas.js" id="Snippet_UseSas":::


## See also

- [Types of SAS tokens](../common/storage-sas-overview.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json)
- [How a shared access signature works](../common/storage-sas-overview.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#how-a-shared-access-signature-works)
- [API reference](/javascript/api/@azure/storage-blob/)
- [Library source code](https://github.com/Azure/azure-sdk-for-js/blob/main/sdk/storage/storage-blob)
- [Give Feedback](https://github.com/Azure/azure-sdk-for-js/issues)