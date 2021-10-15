---
layout: post
title:  "How to Set HTTP Headers When Uploading Files Using Angular 7 and the Azure Storage SDK V10 for JavaScript Library"
date:  2019-02-17
categories: [technology]
images: michaeldeongreen-logo.png
tags: [angular-cli, Angular 7, Azure Storage]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In my previous blog [How to Upload Files to Azure Storage Using Angular 7 and the Azure Storage SDK V10 for JavaScript Library](https://blog.michaeldeongreen.com/technology/2019/01/19/how-to-upload-files-to-azure-storage-using-angular-7-and-the-azure-storage-sdk-v10-for-javascript-library.html), I demonstrated how to upload files to Azure Storage using Angular 7 and the Azure Storage SDK V10 for JavaScript Library.  
  
In this blog, I want to build on top of the previous blog and demonstrate how to how to set HTTP Headers when uploading files using Angular 7 and the Azure Storage SDK V10 for JavaScript Library.  
  
**Prerequisites!**  
  
* All the prerequisites listed in the [previous](https://blog.michaeldeongreen.com/technology/2019/01/19/how-to-upload-files-to-azure-storage-using-angular-7-and-the-azure-storage-sdk-v10-for-javascript-library.html) blog

**Background!**  
  
When uploading files using the [uploadBrowserDataToBlockBlob](https://docs.microsoft.com/en-us/javascript/api/@azure/storage-blob/?view=azure-node-preview#uploadbrowserdatatoblockblob-aborter--blob---arraybuffer---arraybufferview--blockbloburl--iuploadtoblockbloboptions-) method for parallel uploading, there are only 6 HTTP Request Headers that you can set using the [BlobHTTPHeaders](https://docs.microsoft.com/en-us/javascript/api/%40azure/storage-blob/blobhttpheaders?view=azure-node-preview) attribute of the [IUploadToBlockBlobOptions](https://docs.microsoft.com/en-us/javascript/api/@azure/storage-blob/IUploadToBlockBlobOptions?view=azure-node-preview) interface.  
  
Recently, one of our Client Partners at Microsoft recently asked me if it was possible to set the [client-request-id](https://msdn.microsoft.com/en-us/library/dn364662.aspx) HTTP Request Header and the purpose of this blog is to demonstrate how this can be done using the existing codebase that we used in the previous blog, which you can find [here](https://github.com/michaeldeongreen/angular7-azure-storage-sdk-v10-demo).  
  
**Setting the client-request-id per upload!**  
  
First, we need to create a _Request Policy_ to set the [client-request-id](https://msdn.microsoft.com/en-us/library/dn364662.aspx). The new policy class for the [client-request-id](https://msdn.microsoft.com/en-us/library/dn364662.aspx) HTTP Header will allow the [client-request-id](https://msdn.microsoft.com/en-us/library/dn364662.aspx) to be set per request.  
  
Create the new Request Policy by typing the following:  

 ```bash
    ng g class azure-storage/clientRequestIDPolicy
```

Next, open up the new class and paste the following code:  

```typescript  
    import { BaseRequestPolicy, RequestPolicy, RequestPolicyOptions, WebResource, HttpOperationResponse } from '@azure/ms-rest-js';
    
    export class ClientRequestIDPolicy extends BaseRequestPolicy {
        private clientRequestId: string;
    
        constructor(nextPolicy: RequestPolicy, options: RequestPolicyOptions, clientRequestId: string) {
            super(nextPolicy, options);
            this.clientRequestId = clientRequestId;
          }
        
          async sendRequest(webResource: WebResource): Promise {
            const CLIENT_REQUEST_ID = 'client-request-id';
            webResource.headers.set(CLIENT_REQUEST_ID, this.clientRequestId);
        
            const response = await this._nextPolicy.sendRequest(webResource);
            return response;
          }    
    }
```

The class above implements the Azure Storage SDK V10 for JavaScript _BaseRequestPolicy_ interface. One of the parameters for the constructor is the _client-request-id_. The _sendRequest_ method is an implementation of the _BaseRequestPolicy_ interface and is the piece of code that sets the _client-request-id_ HTTP Header per upload.  
  
Next, we need to create the _Request Policy Factory_ that will be used to call the _ClientRequestIDPolicy_ class.  
  
Type the following:

```bash
    ng g class azure-storage/clientRequestIDPolicyFactory
```

Next, open up the new class and paste the following code:  
  
```typescript  
    import { RequestPolicy, RequestPolicyOptions, RequestPolicyFactory } from '@azure/ms-rest-js';
    import { ClientRequestIDPolicy } from './client-request-idpolicy';
    
    export class ClientRequestIDPolicyFactory implements RequestPolicyFactory {
        private clientRequestId: string;
        constructor(clientRequestId: string) {
          this.clientRequestId = clientRequestId;
        }
      
         create(nextPolicy: RequestPolicy, options: RequestPolicyOptions): ClientRequestIDPolicy {
           return new ClientRequestIDPolicy(nextPolicy, options, this.clientRequestId);
         }    
    }
```

The class above implements the Azure Storage SDK V10 for JavaScript _RequestPolicyFactory_ interface. The constructor accepts the _client-request-id_ as a parameter and implements the _create_ method of the _RequestPolicyFactory_ interface. The _create_ method will instantiate our new _ClientRequestIDPolicy_ class where it will set the _client-request-id_.  
  
Next, we need to make a few small modifications to our _BlobStorageService_ service.  
  
First, you will want to import the new _ClientRequestIDPolicyFactory_ class:  
  
```typescript  
    import { ClientRequestIDPolicyFactory } from './client-request-idpolicy-factory';
```

Next, in the _uploadBlobToStorage_ method, after the line below:  

```typescript  
    const pipeline = StorageURL.newPipeline(anonymousCredential);
```

Add the following line of code to call the _ClientRequestIDPolicyFactory_ class to set the _client-request-id_:  

```typescript  
    pipeline.factories.unshift(new ClientRequestIDPolicyFactory('dba75b71-a943-4532-86be-07f86b1e78f0'));
```

The new line of code above will create a new _ClientRequestIDPolicyFactory_ class and pass a GUID to the constructor. Also note, for brevity, I have hardcoded a GUID value but for _Production Level_ code, you will want to research effective ways to generate GUIDs in JavaScript or use a Middle-Tier API to retrieve a GUID.  
  
**Upload a file and view the client-request-id!**  
  
Now, upload a file and view the network activity after the file has been uploaded. Find the _last_ uploaded activity, which is the final _BlockBlob_ and you should see that the _client-request-id_ HTTP Header has been set to the hardcoded GUID value:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-set-http-headers-when-uploading-files-using-angular-7-and-the-azure-storage-sdk-v10-for-javascript-library-001.png)
