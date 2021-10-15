---
layout: post
title:  "Odd Azure Storage, Azure Portal and Visual Studio 2015 Behavior"
date:  2017-02-22
categories: [technology]
images: michaeldeongreen-logo.png
tags: [azure, career advice, azure cloud service, visual studio]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

This week I decided to devote some time to learn a little more about Azure Cloud Services. To begin educating myself, I decided to try my hand at implementing the outstanding [Contoso Ads Demo Application](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-get-started). You can find my completed solution on GitHub [here](https://github.com/michaeldeongreen/ContosoAds).  
  
I recreated the entire Contoso Ads Application from scratch, but when I was ready to deploy the Cloud Service to Azure, I observed some very odd behavior. I think this behavior is based upon whether you create the Azure Storage Account in Azure Classic vs Azure Resource Manager mode.  
  
The first odd behavior that I found was that when I tried to set the Web and Worker Role Azure Storage and Diagnostic Cloud Connection Strings in the Visual Studio 2015 Settings window under each role's property page; the Azure Storage Account Name would not display in the drop down box.  
  
![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-001.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-001.png)  
  
After I observed this behavior, I immediately went to the Azure Portal to ensure that I had created the Azure Storage Account correctly and everything was in order. I then navigated to the Cloud Explorer in Visual Studio 2015 to see if the Azure Storage Account appeared there, and it did.  
  
![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-003.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-003.png)  
  
Next I tried to enter the Azure Storage Account information manually in the Create Storage Connection String screen in the Web and Worker Role properties settings.  
  
![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-004.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-004.png)  
  
Unfortunately, when I tried to publish the Cloud Service to Azure in the Visual Studio 2015 Publish Wizard, I was prompted to create an Azure Cloud Service. This was because Visual Studio 2015 was not able to see the Azure Storage Account I created in the Azure Portal� I was basically back to my original problem.  
  
After doing some research and pondering why I was having this issue, I remembered that when I created the Azure Storage Account, there was an option to use either Resource manager or Classic for the Deployment model. So I decided to delete the Azure Storage Account and re-create it using the Classic deployment model option. ![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-002.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-002.png)  
  
After I re-created the Azure Storage Account, much to my surprise, Visual Studio 2015 was able to find the Azure Storage Account in the Create Storage Connection String screen.  
  
![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-005.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-005.png)  
  
I was finally able to deploy the Contoso Ad Cloud Service to Azure without any issues using Visual Studio. After deploying my service, I created another Azure Storage Account using the Resource manager deployment option to see if Visual Studio 2015 would see it, and of course it did not. However, I noticed another odd set of behaviors in the Azure Portal: both Azure Storage Accounts displayed properly in the Azure Portal under the All Resources tab.  
  
![michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-006.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-006.png)  
  
However, when I navigated to the Storage accounts tab in the Azure Portal, only the Azure Storage Account created with the Resource manager deployment option would display.  
  
![assets/images/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-007.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-odd-azure-storage-azure-portal-and-visual-studio-2015-behavior-007.png)  
  
**Conclusion!**  
  
I am sure that the behavior that I describe as "odd" is a known issue by Microsoft, and most likely it is behavior that was implemented (or not implemented) by design. It is my understanding that the Azure Portal was completely re-done over the last few years, and as a result, the Azure Team probably had to make some compromises to retrofit some of the Azure Resources that were created in the Azure Classic Portal into the new Azure Portal. I just wanted to write a quick blog talking about the issues I faced, in case others run into the same problems and to point out that right now, there is some confusion around Azure Classic and the current Azure Portal.
