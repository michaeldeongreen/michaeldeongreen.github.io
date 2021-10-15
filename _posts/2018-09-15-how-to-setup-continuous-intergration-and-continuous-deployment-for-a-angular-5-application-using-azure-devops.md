---
layout: post
title:  "How to Setup Continuous Intergration and Continuous Deployment for a Angular 5 Application using Azure Devops"
date:  2018-09-15
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Angular 5, angular-cli, Azure, Azure DevOps, GitHub, NPM]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In this blog, I want to demonstrate how to setup Continuous Integration (CI) and Continuous Deployment (CD) for a Angular 5 application using [Azure DevOps](https://azure.microsoft.com/en-us/blog/introducing-azure-devops/). I will do this by using [GrenitausConsulting.com](http://grenitausconsulting.com/), which is my custom built web application that uses Angular 5 and is already deployed to Azure, however, at this time, it is deployed manually via FTP. The Github Repository can be found [here](https://github.com/michaeldeongreen/GrenitausConsulting)  
  
**Caveats!**  
  
Also, I assume that you are familiar with:  
  
* Angular 5+
* angular-cli
* Azure Portal
* GitHub
* Node Package Manager

[GrenitausConsulting.com](http://grenitausconsulting.com/) is already hosted in the Azure Cloud via a Azure Web App, so I just needed to setup Continuous Integration and Continuous Deployment in VSTS, which is now called [Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/user-guide/what-happened-vsts?view=vsts).  
  
**Azure DevOps CI & CD Project Setup!**  
  
First, I navigated to my VisualStudio.com url _(Ex: \[yourdomain\].visualstudio.com)_ to start creating a Azure DevOps Project for [GrenitausConsulting.com](http://grenitausconsulting.com/).  
  
Now that I am in the VisualStudio.com portal, I created a new project:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-01.png)  
  
Once the project is created, on the left navigation menu, I chose "Builds" and created a new Pipeline and I used a [GitHub personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-02.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-03.png)  
  
Once this is done, I pressed "continue" and chose "Empty Pipeline". On the main Pipeline screen, I selected the "Triggers" tab and under Continuous Integration, I chose "Enable continuous integration":  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-04.png)  
  
Next, under the "Tasks" tab, I chose "Agent Job 1" and re-named it to something more descriptive _(I chose Build & Deploy)_  
  
After I made this change, in the tab section, I chose "Save" _(Something you will want to do periodically)_ and when the popup appeared, I just pushed the "Save" button:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-05.png)  
  
Next, on the "Agent Job", I selected the "+" sign, which will allow you to create tasks for your job.  
  
For my first task, I needed to install node package dependencies. So I did a search for "npm" and selected the "npm" task. Inside of the "npm" task, I set the "Working folder with package.json":  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-06.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-07.png)  
  
Next, I needed to create a "Command Line" task to run [angular-cli](https://cli.angular.io/). Once again, I pressed the "+" sign next to the Agent Job, did a search for "Command Line" and added the "Command Line" task. Once inside the task, I typed "npx ng build" in the script window. For more inforamtion on "npx" see [How to use angular-cli locally](https://medium.com/@starikovs/how-to-use-angular-cli-locally-729dbb6707dd).  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-08.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-09.png)  
  
After I added this task, I added the "Azure App Service Deploy" task, which will be responsible for deploying my changes to Azure. Inside of this task, I chose my Azure Subscription, my App Type and set the "dist" directory that angular-cli should have created during the "Command Line" task:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-10.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-11.png)  
  
After I created all of my tasks, I wanted to test the CI & CD Process. I did this by selecting the "Queue" option on the Pipeline screen.  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-12.png)  
  
On the left navigation menu, I chose "Builds". Once there, I chose the latest build and this allowed me to observe the output from each Agent Job Step I had just configured. Once the Agent Job completes, all of the task steps should have a status of "succeeded":  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-13.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-14.png)  
  
After this, I navigated to [GrenitausConsulting.com](http://grenitausconsulting.com/) to ensure that it was up and running:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-15.png)  
  
After this, I wanted to make sure that when I checked a change into the "master" branch that the "Build & Deploy" Agent Job would be triggered. So I made a trivial change and pushed the changes to GitHub. Once I had done this, I verified that there was a new build and that all the tasks completed successfully.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-setup-continuous-intergration-and-continuous-deployment-for-a-angular-5-application-using-azure-devops-16.png)  
  
**Conclusion!**  
  
I have always wanted to automate the build and deployment for some of my open source projects. When I learned about Azure DevOps and that it was basically [free](https://azure.microsoft.com/en-us/blog/announcing-azure-pipelines-with-unlimited-ci-cd-minutes-for-open-source/) for Open Source Projects, I jumped at the opportunity. I felt that Azure DevOps was fairly straightforward, intuitive and I will definitely be using it in the future.  
  
Next, I will start the process of automating CI and CD for [Blog.Michaeldeongreen.com](http://Blog.Michaeldeongreen.com/), which is going to be far more complicated and of course, I will share my experience in a blog entry.
