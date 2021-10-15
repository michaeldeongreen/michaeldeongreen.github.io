---
layout: post
title:  "Angular 2 Continuous Deployment to Azure using azure-cli and GitHub"
date:  2016-11-06
categories: [technology]
images: azure-angular.png
tags: [angular 2, azure web app, azure-cli, git bash, git, github, typescript]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/azure-angular.png)

In this blog, I wanted to demonstrate how to setup Angular 2 Continuous Deployment to Azure using azure-cli and Github. I am not an expert on Azure or Angular 2 but when I attempted to setup continuous deployment for the Angular 2 QuickStart guide to Azure using GitHub, I felt that the experience was not very intuitive, so I decided to write a blog about my experiences.  
  
**What you will need**  
  
* A Azure Account
* A Github Account
* Git
* Node Package Manager
* Visual Studio 2015

**Assumptions**  
  
* You have basic knowledge of Azure Web Apps, Git, Github, Node Package Manager and Visual Studio
* You have the proper access and permissions to the Azure Portal and Github
* You have completed the [Angular 2 Visual Studio 2015 QuickStart](https://angular.io/docs/ts/latest/cookbook/visual-studio-2015.html)

**Step 1 - GitHub**  
  
* Ensure that the solution file (.sln), the (.csproj) and the Angular 2 QuickStart files are in the root directory. Later in this blog, I discuss why I needed to set my project up this way. Your root directory should look like the picture below:
  
[![grenitausconsulting-com-angular-2-continuous-deployment-with-azure-and-github-001](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-001.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-001.png)  
  
* If you haven't already, create a Github repository and push your Angular 2 QuickStart Project to Github. Your Github repository should look similar to the picture below (_ignore the .deployment and deploy.cmd files for now_):
  
[![grenitausconsulting-com-angular-2-continuous-deployment-with-azure-and-github-002](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-002.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-002.png)

**Step 2 - Azure Portal**  
  
* First, log into the [Azure Portal](https://portal.azure.com)
* Expand the menu on the left, choose App Services and on this screen, click the Add button at the top
* Once the App Services Screen loads, under Web Apps, choose Web App and on the bottom right, click the Create button
* On the Web App Screen, enter the App name, your Subscription, the Resource Group and your App Service Plan and click the Create Button. If you are unfamiliar with these options, you can view the official Microsoft documentation [here](https://azure.microsoft.com/en-us/documentation/services/app-service/web/)
* Next, when you click on All Resources on the left menu, you should see your new Web App. I have called my Web App AngularIOQuickStartWeb and you can see it below:
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-003.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-003.png)  
  
* Next, click your Web App, click Deployment Options, Choose Source and select GitHub. Enter your GitHub Authorization, Choose your GitHub Project and Choose your branch
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-004.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-004.png)  
  
* Once you have setup Continuous Deployment for the first time, Azure will automatically build and deploy your project.
* To see the results of the build and deployment, click on your Web App, choose Deployment options, click on the log item (middle pane), then click the View Log, to see the build and deployment log
  
* You should notice that the build failed and you have many errors in your log. Below is my log:
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-005.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-005.png)

**Step 3 - Why did my Azure Continuous Deployment Fail, Round 1**  
  
* If you look closely at the Deployment log, you will see that many of the errors are TypeScript errors. As it turns out, we need to somehow install our node modules in the Cloud. The way we do this is with the handy [Azure CLI](https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/) tool. Azure-cli will create a custom deploy.cmd deployment script that will run during Continuous Deployment
  
* Next, navigate to the root folder of your QuickStart Project and open up a terminal (I am using Git Bash) and type in the following:
  
  ```bash
    npm install -g azure-cli

    ```

* Next, we need to associate the azure-cli tool with your Azure account. We do this by typing the following:

    ```bash
        azure login
    ```

* Once you have typed this in, it should output a code and the website you will use to enter the code.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-006.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-006.png)  
  
* Once you have entered the code, you should see output similar to the following:
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-007.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-007.png)  
  
* Next, type in the below in your cli to switch azure-cli to [Service Management Mode](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-command-line-tools/):

```bash
    azure config mode asm
```

[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-008.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-008.png)  
  
* After you have done this, type the following command:

```bash
    azure site deploymentscript --node
```

* In the root folder of your project, you should now see 2 new files, .deployment and deploy.cmd.
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-009.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-009.png)  
  
* Open deploy.cmd, under the "**:Deployment**" section, right below the "**Install npm packages**" section, place the following:
  
**4. Compile TypeScript**

```bash
    echo Transpiling TypeScript in %DEPLOYMENT_TARGET%...
    call :ExecuteCmd node %DEPLOYMENT_TARGET%\node_modules\typescript\bin\tsc -p "%DEPLOYMENT_TARGET%"
```

The section you just modified should like similar to the picture below: [![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-010.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-010.png)  
  
* Jeremy Foster has written a very good blog that goes into great detail about the deploy.cmd file and the changes that we have made. You can find his blog [here](http://www.codefoster.com/tscazure/). To summarize, we have created a custom deploy.cmd deployment script that Azure will now execute as part of the deployment process. If a package.json file exists, our node modules will be installed. Also, the script that we added ensures that any typescript files that are in the Cloud will be transpiled into JavaScript.
  
* After we have made these changes, let's push them to GitHub
  
* If you have a similar version of the Angular 2 QuickStart source code that I do, when you view the latest deployment log in azure, you should notice that although it states that the deployment was successful in the middle pane, the log has errors and should look like below:
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-011.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-011.png)  
  
* Why are we getting this error? Head to the next step and you will find out why!
  
**Step 4 - Why did my Azure Continuous Deployment Fail, Round 2**  
  
* If you look closely at the error in the latest deployment log, it is literally stating that it cannot find the typescript folder that should be in the node\_modules folder. Also, when I navigated to the deployed website, which you can do by clicking on your Web App, click Overview and click the URL link, I had a very interesting JavaScript error that gave even more insight into the issue I was having.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-018.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-018.png)  
  
* When I first encountered this problem, the first thing that popped into my head was, "I would like to see what has been deployed to the Cloud." The way you can do this is by logging into your Cloud server via FTP.
* In the Azure Portal, in the left menu, click on All resources, click on your Web App, click on Deployment credentials. On this screen, enter a username and password that you would like to use to FTP into the Cloud server.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-012.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-012.png)  
  
* Next, under your Web App, scroll down and find Diagnostics logs. On this screen, you can turn on logging and it also has the FTP, FTPS (_I used this option_), full username and path that we need to access the Cloud server
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-013.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-013.png)  
  
* I use the free FTP Client FileZilla to access FTP locations but you are free to use whatever client to access your Cloud server. You can find FileZilla [here](https://filezilla-project.org/download.php?type=client)
* When I accessed the Cloud server, navigated to wwwroot and looked inside of the node\_modules folder, sure enough, the typescript folder had not been the deployed.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-014.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-014.png)  
  
* So why hasn't the typescript node module been deployed? It turns out that the Angular 2 QuickStart source that I downloaded from their official GitHub site is missing an entry in the package.json.
* In your Visual Studio Project, open the package.json file and notice that the "devDependencies" section has an entry for typescript. However, if you view the "dependencies" section of the package.json file (_which is used for Production_), there is no entry for typescript. So we just need to copy the below into the dependencies section:

```bash  
    "typescript": "^2.0.3"
```

Your dependencies section should look similar to the picture below:  
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-015.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-015.png)  
  
* After we push our changes to GitHub and view the deployment log, you should see that the previous error about not being able to find the typescript folder has gone away and there should be a part of the deployment log that denotes that the typescript (.ts) files are being transpiled. Also, if you re-log into the Cloud server, you should see the typescript folder in the node\_modules folder.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-016.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-016.png)  
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-017.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-017.png)  
  
* When you navigate to your Azure website, the Angular 2 QuickStart should load. Below is my Angular 2 QuickStart in the Cloud.
  
[![angular 2 continuous deployment to azure](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-019.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting.com-angular-2-continuous-deployment-with-azure-and-github-019.png)

**Troubleshooting**  
  
Admittedly, deploying the Angular 2 QuickStart tutorial to the Azure Cloud was a little tougher than I thought it would be. One of the major issues that I faced was that the latest Angular 2 QuickStart source code would not compile. I was under the impression that as long as you had installed the latest version of typescript globally using npm and you went into the Visual Studio Options and under External Web Tools, have placed the PATH variable above the DevEnvDir variables, that your PATH Environment variables would be evaluated first.  
  
Sadly, this was not the case. Visual Studio is still going to use the version of typescript that is in "**C:\\Program Files (x86)\\Microsoft SDKs\\TypeScript**". Allen Conway wrote a great blog about this issue, which you can find [here](http://www.allenconway.net/2015/07/which-version-of-typescript-is.html). Once I installed the latest version of typescript (_at the time of this blog, 2.0.7_) for Visual Studio, which you can find [here](https://www.microsoft.com/en-us/download/details.aspx?id=48593), the Angular 2 QuickStart finally compiled.  
  
Another issue that I faced was that when I originally created the Visual Studio Project for the Angular 2 QuickStart by following the instructions on the Angular.io website, the solution file (.sln) was in its own folder in the root and the project (.csproj) file and all the QuickStart files were in a sub folder. After I had deployed the Angular 2 QuickStart guide to Azure, I got "**You do not have permission to view this directory or page**". Under your Web App settings, under Application settings, Azure has default pages configured but if the index.html page in the QuickStart tutorial is not in the root directory, you will get the permission error that I received. This is why I stated at the beginning of the tutorial that the QuickStart files should be in the root directory.  
  
**Pros**  
  
* **Logging** - As someone who has spent many restless nights troubleshooting production issues, I am a huge fan of logging. I really like how descriptive the Deployment options logs are and that you can also access the logs via FTP.
* **Information is Readily Available** - Prior to this tutorial, I had not logged into the Azure Portal in a very long time and the site has changed a lot since then. Also, I am not an expert on Azure, hence the reason I wanted to start traveling down that road. I really liked that it was fairly simple to set GitHub and FTP credentials and the information I needed to actually FTP into my Cloud server was easy to find.

**Cons**  
  
* **Continuous Deployment** - I was a little surprised that Azure Continuous Deployment was really Continuous Integration + Continuous Deployment. I suspect that when some people see Continuous Deployment, they will assume that no compiling or installs will occur, only the needed bits will be deployed to the Cloud. In my opinion, there should a separate step for Continuous Integration and a separate step for Continuous Deployment, where only the Html, JavaScript and relevant .Net libraries are deployed.
* **Root Directory** - As I mentioned earlier, I initially had the Angular 2 QuickStart files in a sub directory. Azure Continuous Deployment checks out your source code from GitHub and places it on the Application Server in the same structure as it is on GitHub. If the Angular 2 QuickStart files are not in the root folder, you will get permissions errors. I suspect that the deploy.cmd script can be used to clean up and restructure the deployment directory.
* **Pre-Deployment Cleanup** - On a few occasions, I noticed that deleted/modified files from previous deployments were still on the server. On 2 occasions I had to FTP into the Cloud server and delete the entire root directory to remove files that I had deleted/modified on GitHub

**Conclusion**  
  
Although there were some challenges when trying to deploy my first Angular 2 application to Azure, I felt it was fun, informative and time well spent. I hope you all found this blog post informative.
