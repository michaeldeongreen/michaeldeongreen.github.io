---
layout: post
title:  "How to Locally Debug a ASP.Net Core Web API that Receives Messages from Azure EventGrid"
date:  2019-09-13
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Net Core, ASP.Net Web API, Azure EventGrid]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

At one our Client Partners here at Microsoft, we are using the [Azure EventGrid](https://azure.microsoft.com/en-us/services/event-grid/) to power our event driven application written in ASP.Net Core. In this blog, I wanted to take a moment to demonstrate how to locally debug a ASP.Net Core Web API that receives messages from Azure EventGrid.  
  
In this tutorial, I will demonstrate how to debug a ASP.Net Core Web API endpoint that receives messages when a new blob is created in an Azure Blob Storage Account. The code used in this tutorial can be found [here](https://github.com/michaeldeongreen/azeventgrid-local-debug-aspnet-core-ngrok)  
  
**Pre-requisites!**  
  
* [.Net Core 2.2](https://dotnet.microsoft.com/download/dotnet-core/2.2)
* An Azure Subscription
* [azure-cli](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
* Git
* [Visual Studio Code](https://code.visualstudio.com/)

**Note:** Before running any of the commands, always ensure you have logged into azure by executing the following command:  
  
        az login

**ngrok**  
  
First, we need to donwload a tool called [ngrok](https://ngrok.com/). Once you create an account, following the instructions on how to register your authtoken.  
  
**Register the Azure EventGrid Provider**  
  
If not already registered, we will need to [register](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart#enable-event-grid-resource-provider) the Azure EventGrid. Open up a terminal and run the following command.  
  
        az provider show --namespace Microsoft.EventGrid --query "registrationState"

**Create Azure Blog Storage Account**  
  
We will clone the [sample application](https://github.com/michaeldeongreen/azeventgrid-local-debug-aspnet-core-ngrok) and navigate to the _scripts_ folder and execute the following command to create our Azure Blog Storage Account and a Blob container called _demo_:  
  
        chmod +x create-az-storage-account.sh &&
        ./create-az-storage-account.sh -r {name_of_resource_group} -l {location} -a {name_of_blob_storage_account}

If the script ran successfully, it will return back the message _The script has created the Azure Resources_.  
  
**Debugging!**  
  
Open up the ASP.Net Core Web API project in Visual Studio Code and press _F5_ to run the project. A quick note, for Ubuntu, I had to modify the _launchSettings.json_ file to use _http_ to allow ngrok to work.  
  
In the _EventGridController_, place breakpoints in various places to debug the application when a message is received from Azure EventGrid.  
  
Next, we need to run ngrok. We do this by executing the command below: _(note: for Windows user, you need to use the command prompt as I could not get git bash to work)_.  
  
        ngrok http -host-header=localhost 5000

Next, we will need to copy the generated url so we can use it to subscribe to events via Azure EventGrid.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-001.png)  
  
Next, we need to navigate to the [Azure Portal](https://https://portal.azure.com) to create the Subscription and upload a file, so we can debug the application.  
  
In the Azure Portal, we need to find the Azure Blob Storage Account we created when we ran the _create-az-storage-account.sh_ bash script. Select "Events" and then "Event Subscription".  
  
For the _Endpoint Type_, we will choose "webhook" and copy the url ngork generated and add "/eventgrid" to the end ie _<https://ngrok-url/eventgrid>_. See the images below for guidance:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-002.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-003.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-004.png)  
  
When we hit the _create_ button to create the Subscription, for security reasons, Azure EventGrid will validate the endpoint and if you have a breakpoint similar to the picture below, the breakpoint should be activated in Visual Studio Code:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-005.png)  
  
Also, the new Subscription should be displayed on the Events page of the Azure Blob Storage Account:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-006.png)  
  
Now let's upload a file and debug the _HandleEventGridEventAsync_ Method inside of the EventGridController _(make sure you have a breakpoint in the HandleEventGridEventAsync method)_.  
  
In the Azure Portal, we need to find the Azure Blog Storage Account and navigate to the _demo_ container. Inside of the container, we will press the _upload_ button. Included in the Git repository root directory is a file called _sample.json_, you can choose this file or a file of your own choosing. For guidance, see the pictures below:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-007.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-008.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-009.png)  
  
Once the file is finished uploading, our breakpoint should be activated in Visual Studio Code:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-locally-debug-a-asp-net-core-webapi-that-receives-messages-from-azure-eventgrid-010.png)  
  
**Clean Up!**  
  
Now that we are finished, we can run the _delete-az-storage-account.sh_ bash script to clean up our resources. Run the following command:  
  
```bash
    
        chmod +x delete-az-storage-account.sh &&
        ./delete-az-storage-account.sh -r {name_of_resource_group}
```

If the script ran successfully, it will return back the message _The script has deleted the Azure Resources_.
