---
layout: post
title:  "How to Run Docker on a Windows 10 Pro Hyper-V Virtual Machine"
date:  2019-04-06
categories: [technology]
images: michaeldeongreen-logo.png
tags: [docker, Hyper-V, Virtual Machine]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

I wanted to write a very quick blog on how to run docker on a Windows 10 Hyper-V Virtual Machine.  
  
**Caveats!**  
  
This blog entry is not a tutorial on Docker, Hyper-V or Powershell. The expectation is that you already have basic knowledge about these topics and concepts.  
  
**Prerequisites!**  
  
* Hyper-V
* Powershell
* Windows 10 Pro Hyper-V Host VM
* Windows 10 Pro Hyper-V Guest VM

**Background!**  
  
As I have progressed through my consulting career, one of the skills I have started to employ is to use Hyper-V to create a VM for each of my projects to avoid having to install a bunch of software directly on my laptop. Lately, I have been using Ubuntu VMs over Windows VMs for a variety of reasons and docker works perfectly fine running on an Ubuntu 18.04 Hyper-V VM.  
  
On my last project here at Microsoft, our Client Partner had a _hard_ dependency on Windows and I quickly found out that running docker on a Windows Hyper-V VM was non-trivial (or at least it appeared this way at the time). Due to a lack of time and knowledge, I had to shelf learning how to get docker to work inside of a Windows Hyper-V VM.  
  
On my current project, we are working a lot with docker and with some guidance from my co-workers, I was able to learn how to get docker up and running on a Windows Hyper-V VM and it turned out to be fairly trivial.  
  
**Let's Get Started!**  
  
First, navigate to Microsoft Docs GitHub repository for virtualization and get the Powershell script that can be used to enable [Nested Virtualiztion](https://www.webopedia.com/TERM/N/nested-virtualization.html), which can be found [here](https://github.com/MicrosoftDocs/Virtualization-Documentation/blob/live/hyperv-tools/Nested/Enable-NestedVm.ps1).  
  
Copy this Powershell script to a folder on your computer.  
  
Next, open up Hyper-V and do the following:  
  
* Ensure that your Windows VM is not running
* Get the name of the Windows VM

Next, open up Powershell as an administrator, navigate to the directory where you downloaded the _Enable-NestedVm.ps1_ Powershell script and run the following command (_Enter "Y" for all question prompts_):  
  
```powershell
    .\Enable-NestedVm.ps1 {name-of-windows-vm}
    
```
  
In Hyepr-V, start your vm and install [docker docker for windows](https://docs.docker.com/docker-for-windows/install/). After docker for windows has been installed, you will need to logout of the VM.  
  
Once you have logged back into your Windows VM, you will be prompted with the following (_which you will select Ok_):  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-run-docker-on-a-windows-10-pro-hyper-v-virtual-machine-001.png)  
  
After docker for windows is installed, the VM will restart and will take several minutes for docker to start.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-run-docker-on-a-windows-10-pro-hyper-v-virtual-machine-002.png)  
  
Once docker for windows is finished loading, you will want to ensure docker for windows is working correctly inside of the Windows 10 Hyper-V VM by pulling down the _hello-world_ image and running it. Open a bash prompt and type the following:  
  
```bash
    
    docker pull hello-world && docker run hello-world
```

If docker is installed correctly, you should see the following:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-run-docker-on-a-windows-10-pro-hyper-v-virtual-machine-003.png)
