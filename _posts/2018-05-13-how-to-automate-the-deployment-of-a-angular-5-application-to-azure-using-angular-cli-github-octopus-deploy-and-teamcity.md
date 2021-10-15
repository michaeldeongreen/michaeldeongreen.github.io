---
layout: post
title:  "How to Automate the Deployment of a Angular 5 Application to Azure using angular-cli, GitHub, Octopus Deploy and TeamCity"
date:  2018-05-13
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Azure, Azure Web App, azure-cli, Angular 2, Git Bash, GitHub, Octopus Deploy, TeamCity]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In this blog, I wanted to do a quick write-up on how to automate the deployment of a [Angular 5](https://angular.io/) application to [Azure](https://azure.microsoft.com/) using [angular-cli](https://cli.angular.io/), [GitHub](https://github.com/), [Octopus Deploy](https://octopus.com/) and [TeamCity](https://www.jetbrains.com/teamcity/).  
  
**Caveats!**  
  
In this blog, I assume that you are familiar with:  
  
* Angular 5+
* angular-cli
* Azure
* GitHub
* Node Package Manager
* Octopus Deploy
* Teamcity
* Visual Studio 2017

I also assume that you have the proper Teamcity plugins for [Node.js](https://github.com/jonnyzzz/TeamCity.Node) and [Octopus Deploy](https://octopus.com/downloads) installed.  
  
**Octopus Deploy Part 1**  
  
To get started, you will need to allow Octopus Deploy to access your Azure Subscription and you do this by creating a Azure Active Directory Application and an Azure Service Principal Account. To avoid redundancy, Octopus Deploy has a very easy to follow guide on how to do this and it can be found [here](https://octopus.com/docs/infrastructure/azure/creating-an-azure-account/creating-an-azure-service-principal-account). Once you have done this, move on to the next portion of this blog.  
  
**Create the Sample Angular 5 Application!**  
  
Next, we will want to create a basic Angular 5 application using angular-cli so that we can deploy it to Azure.  
  
1. Open up [Git Bash](http://Blog.Michaeldeongreen.com/post/my-decision-to-trade-in-my-git-gui-client-for-git-bash) and navigate to a directory of your choosing and type:

```bash
            ng new NEWPROJECT
```

3. Open Visual Studio and in the new directory that angular-cli created, make a new blank solution project. Inside of the new solution, add a Class Library (.NET Framework).  
    **Note:** I did this more for organization and to take advantage of [TypeScript](https://www.typescriptlang.org/) compile time checking.
4. Next, you will want to select the Class Library, choose _Show All Files_ in Solution Explorer and add the files and directories that I have in the picture below:
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-001.png)7.  Next, you will want to add a Web.config to the solution and place it in the _src_ root directory. You can use the Web.Config that I created, which you can find on [GitHub](https://github.com/michaeldeongreen/GreenOneAzureOctopusDeploy/blob/master/src/Web.config):
  
9. Next, open up the angular.json file, find the _assets_ property under _build_ and an entry for the Web.config. This will place the Web.config in the dist folder when you build the Angular 5 application using angular-cli
10. Next, you will need to install the [OctoPack](https://www.nuget.org/packages/OctoPack/) NuGet Package, which allow TeamCity to create and push the compiled application to Octopus Deploy.
11. Last, compile the solution in Visual Studio and run the following commands in Git Bash to compile and run the Angular 5 application:
  
```bash  
            ng build && ng serve
```

If the Angular 5 application compiles, you should be able to navigate to _<http://localhost:4200>_ and see the default Angular 5 application. You will now want to create a GitHub repository for the new project and commit all changes. The GitHub repository I created for this blog can be found [here](https://github.com/michaeldeongreen/GreenOneAzureOctopusDeploy).  
  
**Create the TeamCity Project!**  
  
As stated, I assume you are familiar with TeamCity. You will want to create a new TeamCity Project and a new Build Configuration. You will want to configure the Project and Build to monitor the GitHub repository you just created.  
  
Now, it is time to create the Build Configuration Steps:  
  
1. Restore NuGet Packages:  
    ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_postss/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-002.png)
2. Run NPM:  
    ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-003.png)
3. Build Solution: ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-004.png)
4. Build and Create Angular Dist.  
    **Note:** This step will create the _dist_ folder, which is the compiled release assets that you will deploy to Azure. The Node.js command _run-script build_ will trigger the angular-cli _ng build_ command. See [Stackoverflow](https://stackoverflow.com/questions/40920471/angular-cli-build-ng-build-on-teamcity/43444190#43444190) for more details:  
    ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-005.png)
5. ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-006.png)
6. ![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-007.png)

That is pretty much it for TeamCity.  
  
**Create the Azure Web App!**  
  
Next, you will want navigate to the Azure Portal and create an Azure Web App. Once you create the Azure Web App, you will want to navigate to the Azure Web App's Diagnostics logs blade and get the FTP information, so you can use an FTP client such as [FileZilla](https://filezilla-project.org/) to verify that Octopus Deploy deployed your Angular 5 application correctly.  
  
**Octopus Deploy Part Deux!**  
  
The final step is to create a new Octopus Deploy Project that will deploy your Angular 5 application to Azure via the Azure Web App you just created.  
  
1. In Octopus Deploy, create a a new Octopus Deploy Project.
2. Once the new project is created, choose _Process_, choose _Add Step and choose the_Deploy an Azure Web App_template_
3. Fill in the Step Name
4. Under Package, the Package ID will be the name of your new Octopus Deploy Project
5. Under Azure, choose the Account that you created when you created the Azure Active Directory Application and the Service Principal using Octopus Deploy's documentation I mentioned earlier in this blog entry. In the Web App dropdown, you should see the Azure Web App you created.

Here is what my process looked like when I was done:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-008.png)  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-009.png)  
  
**Let's Deploy!**  
  
Before we deploy, let's recap what we have done:  
  
* Created a Azure Active Directory Application and Service Principal to allow Octopus Deploy to communicate with Azure
* Created a sample Angular 5 Application and pushed the changes to GitHub
* Created a new TeamCity Project to Build and Push the Angular 5 Application to Octopus Deploy
* Created a new Azure Web App, which is where the Angular 5 Application will be deployed
* Created a new Octopus Deploy Project, using one of the built in Azure Templates to deploy the Angular 5 Application to Azure

Let's head back to TeamCity and deploy the Project (alternatively, you could just make a change to the Angular 5 application and push the changes to GitHub). If everythting works out you should see:  
  
In TeamCity:  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-010.png)  
  
In Octopus Deplpoy:  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-011.png)  
  
In FileZilla (FTP to Azure Web App Site):  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-012.png)  
  
Last but not least, you should be able to view your Angular 5 Application via your Azure Web App url:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-automate-the-deployment-of-a-angular-5-application-to-azure-using-angular-cli-github-octopus-deploy-and-teamcity-013.png)
