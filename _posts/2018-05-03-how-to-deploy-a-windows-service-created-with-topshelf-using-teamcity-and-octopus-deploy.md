---
layout: post
title:  "How to Deploy a Windows Service Created with Topshelf using TeamCity and Octopus Deploy"
date:  2018-05-03
categories: [technology]
images: michaeldeongreen-logo.png
tags: [TeamCity, Octopus Deploy, Windows Service]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

At one of my clients, I recently demonstrated how to use [Topshelf](http://topshelf-project.com/) to create Windows Services and how to deploy the Windows Service using TeamCity and Octopus Deploy. In this blog, I wanted to demonstrate how to use Topshelf to create a Window Service and how to deploy the Windows Service using TeamCity and Octopus Deploy.  
  
**Assumptions!**  
  
* You are already familiar with [TeamCity](https://www.jetbrains.com/teamcity/)
* You are already have a Octopus Deploy Server up and running
* You already have a TeamCity Server version 2017.1+ up and running
* You are already familiar with Windows Services
* You are already familiar with [Topshelf](http://topshelf-project.com/)

**Project Structure!**  
  
I created a simple Windows Service using Topshelf that runs every 10 seconds and writes a list of movies to json files. The code can be found on GitHub [here](https://github.com/michaeldeongreen/TopShelfWindowsService). I started by creating a new Console Application and then installing the [Topshelf](https://www.nuget.org/packages/topshelf) NuGet Package.  
  
Here is my project structure:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-001.png)  
  
Here is my code to configure Topshelf in Program.cs:  

```csharp  
    namespace TopShelfWindowsService
    {
        class Program
        {
            static void Main(string[] args)
            {
                var rc = HostFactory.Run(x =>                                   
                {
                    x.Service(s =>                                   
                    {
                        s.ConstructUsing(name => new MyService());              
                        s.WhenStarted(tc => tc.Start());                        
                        s.WhenStopped(tc => tc.Stop());                         
                    });
                    x.RunAsLocalSystem();
                    x.StartAutomatically();
    
                    x.SetDescription("Sample Topshelf Host");                   
                    x.SetDisplayName("TopshelfWindowsService");                                  
                    x.SetServiceName("TopshelfWindowsService");                                 
                });                                                             
    
                var exitCode = (int)Convert.ChangeType(rc, rc.GetTypeCode());  
                Environment.ExitCode = exitCode;
            }
        }
    }
```

You will need to install the [OctoPack](https://www.nuget.org/packages/OctoPack/) NuGet Package in your Topshelf Console Application to be able to deploy the Windows Service using Octopus Deploy.  
  
**TeamCity!**  
  
In TeamCity, create a new Project and configure the VCS Root:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-003.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-004.png)  
  
Next, create a new Build Configuration for the Project and create 4 Build Configuration Steps:  
  
Build Configuration:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-005.png)  
  
Build steps:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-006.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-007.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-008.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-009.png)  
  
I also added the VCS Root that I created when I created the Project and I added a Trigger to kickoff the Build & Deploy whenever code is checked in to the GitHub repository.  
  
**Octopus Deploy!**  
  
In Octopus Deploy, add a new project, add a step and choose _Deploy a Windows Service_. Below is how I configured the Octopus Deploy Windows Service Template:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-010.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-011.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-012.png)  
  
For the Windows Service: Executable Section, you will need to provide the path to the Windows Service exe and the _servicename_ argument must be the same name you gave your service in the Topshelf Configuration, in my case it is _TopShelfWindowsService_.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-013.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-014.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-015.png)  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-016.png)  
  
Adding my variable I created to change the output directory of the json files.:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-017.png)  
  
**Result!**  
  
Now that I have my Windows Service created and TeamCity and Octopus Deploy setup, I am going to run the project in TeamCity and see if the Windows Service gets deployed and is running correctly.  
  
TeamCity Build & Deploy is successful:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-018.png)  
  
Octopus Deploy has completed successfully:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-019.png)  
  
Checking _Services_ under _Administrative Tools_, I can see that the Windows Service has been installed and is running:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-020.png)  
  
Checking the output directoy, I can see that json files are being generated:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-deploy-a-windows-service-created-with-topshelf-using-teamcity-and-octopus-deploy-021.png)
