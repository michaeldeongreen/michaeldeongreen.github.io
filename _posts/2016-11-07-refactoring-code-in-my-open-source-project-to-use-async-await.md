---
layout: post
title:  "Refactoring Code In My Open Source Project to use async/await"
date:  2016-11-07
categories: [technology]
images: michaeldeongreen-logo.png
tags: [async-await, asynchronous programming, c#, refactoring, task parallel library]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In my last [blog](https://blog.michaeldeongreen.com/technology/2016/11/30/implement-async-wait-in-the-angular-2-tour-of-heroes-web-api.html) I talked about using the async/await asynchronous features in the .Net Framework. I modified one of the Tour of Heroes REST Endpoints to use async/await. In this blog, I will be refactoring code in my open source project to use async/wait instead of the [Backgroundworker](https://msdn.microsoft.com/en-us/library/system.componentmodel.backgroundworker(v=vs.110).aspx).  
  
Background
----------

The open source project that I created roughly 1 year ago is a Windows Form application that allows users to work with Blu-ray discs. Many of the forms use the Backgroundworker to perform long running tasks asynchronous. The part of the application that I wanted to refactor first is the code that makes a call to GitHub to see if a new version of the application has been released and notifying the user.  
  
**Here is what happens:**  
  
1. The user starts the application
2. The main form (MainForm) is displayed
3. The MainForm Form Load event calls a method called CheckForNewVersion
4. The CheckForNewVersion method uses the Backgroundworker to call the AppNotificationService asynchronously
5. The AppNotificationService service contacts GitHub's API to get the latest version of the application
6. After the call completes, if there is a new version of the application, the user is notified via a "new" icon displayed on MainForm

Here is the code before refactoring
------------------------------------

**Backgroundworker Component:**  
  
[![michael-d-green-grenitausconsulting-com-refactoring-code-in-my-open-source-project-to-use-async-await-001](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-refactoring-code-in-my-open-source-project-to-use-async-await-001.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-refactoring-code-in-my-open-source-project-to-use-async-await-001.png)  
  
**MainForm\_Load:**  

```csharp
    private void MainForm_Load(object sender, EventArgs e)
    {
     if (!Program.ErrorLoadingApplicationSettings)
     {
      this.LoadLoggingService();
      this.CheckForNewVersion();
     }
     else
     {
      menuStrip1.SetEnabled(false);
     }
    }
```

**MainForm.CheckForNewVersion:**  

```csharp  
    private void CheckForNewVersion()
    {
     try
     {
      if (Program.ApplicationSettings.CheckForNewVersion)
      {
       IAppNotificationService appNotificationservice = new AppNotificationService(Program.GetApplicationTag());
       bgwCheckForNewVersion.RunWorkerAsync(appNotificationservice);
      }
     }
     catch (Exception ex)
     {
      _loggingService.LogErrorFormat(ex, MethodBase.GetCurrentMethod().Name);
     }
    }
```

**MainForm.Backgroundworker Events:**  

```csharp  
    private void bgwCheckForNewVersion_DoWork(object sender, DoWorkEventArgs e)
    {
     try
     {
      IAppNotificationService appNotificationservice = e.Argument as AppNotificationService;
      AppLatestVersionInfo appLatestVersionInfo = appNotificationservice.GetLatestVersionInfo();
      e.Result = appLatestVersionInfo;
     }
     catch (Exception ex)
     {
      _loggingService.LogErrorFormat(ex, MethodBase.GetCurrentMethod().Name);
     }
    }
    
    private void bgwCheckForNewVersion_RunWorkerCompleted(object sender, RunWorkerCompletedEventArgs e)
    {
     _appLatestVersionInfo = e.Result as AppLatestVersionInfo;
     if (_appLatestVersionInfo != null && _appLatestVersionInfo.IsNewVersion)
     {
      pbNewVersion.Visible = true;
      this.ConfigurepbNewVersion();
     }
    }
```

**AppNotificationService:**  

```csharp  
    public AppLatestVersionInfo GetLatestVersionInfo()
    {
     this.ContactGithubLatestReleaseApi().Wait();
     return _appLatestVersionSettings;
    }
   
    private async Task ContactGithubLatestReleaseApi()
    {
     string latestTag = string.Empty;
     using (HttpClient client = new HttpClient())
     {
      client.DefaultRequestHeaders.Clear();
      client.DefaultRequestHeaders.Add("User-Agent","App");
      client.DefaultRequestHeaders.Accept.Clear();
      client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
      HttpResponseMessage response = await client.GetAsync("githuburl");
      if (response.IsSuccessStatusCode)
      {
       var json = await response.Content.ReadAsStringAsync();
       JObject token = JObject.Parse(json);
       var tagName = token.SelectToken("tag_name");
       var htmlUrl = token.SelectToken("html_url");
       var name = token.SelectToken("name");
    
       _appLatestVersionSettings = new AppLatestVersionInfo() { LatestGithubUrl = htmlUrl == null ? string.Empty : htmlUrl.ToString(),
        TagName = tagName == null ? string.Empty : tagName.ToString(), Name = name == null ? string.Empty : name.ToString()};
    
       if (tagName != null && string.IsNullOrEmpty(tagName.ToString()) == false)
       {
        if (tagName.ToString() != _currentTagName)
         _appLatestVersionSettings.IsNewVersion = true;
        else
         _appLatestVersionSettings.IsNewVersion = false;
       }
       else
        _appLatestVersionSettings.IsNewVersion = false;
      }
     }
    }
```

**Notes about the code above:**  
  
* MainForm.cs uses the Backgroundworker component to run long running processes asychronously
* In the AppNotificationService, there is no error handling. It is up to the calling process to capture the error
* In the AppNotificationService.GetLatestVersionInfo method, it doesn't have the async keyword in the method signature and I am just calling _Wait_ on the ContactGithubLatestReleaseApi Method
* To check for a new application version, I have a Backgroundworker control and 2 Backgroundworker Events

Here is the code after refactoring
-----------------------------------

### AppNotificationService

**AppNotificationService.GetLatestVersionInfo:**  

```csharp  
    public async Task1 GetLatestVersionInfo()
    {
     var task = await this.ContactGithubLatestReleaseApi();
     
    
     return task;
    }
```

**Notes:**  
  
* The method now has an async keyword in the method declaration
* The method returns a Strongly-Typed Task
* The method uses the _await_ keyword

**AppNotificationService.ContactGithubLatestReleaseApi:**  

```csharp  
    private async Task ContactGithubLatestReleaseApi()
    {
     AppLatestVersionInfo appLatestVersionSettings = null;
     string latestTag = string.Empty;
    
     try
     {
      using (HttpClient client = new HttpClient())
      {
       client.DefaultRequestHeaders.Clear();
       client.DefaultRequestHeaders.Add("User-Agent", "App");
       client.DefaultRequestHeaders.Accept.Clear();
       client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
       HttpResponseMessage response = await client.GetAsync("githuburl");
       if (response.IsSuccessStatusCode)
       {
        var json = await response.Content.ReadAsStringAsync();
        JObject token = JObject.Parse(json);
        var tagName = token.SelectToken("tag_name");
        var htmlUrl = token.SelectToken("html_url");
        var name = token.SelectToken("name");
    
        appLatestVersionSettings = new AppLatestVersionInfo()
        {
         LatestGithubUrl = htmlUrl == null ? string.Empty : htmlUrl.ToString(),
         TagName = tagName == null ? string.Empty : tagName.ToString(),
         Name = name == null ? string.Empty : name.ToString()
        };
    
        if (tagName != null && string.IsNullOrEmpty(tagName.ToString()) == false)
        {
         if (tagName.ToString() != _currentTagName)
          appLatestVersionSettings.IsNewVersion = true;
         else
          appLatestVersionSettings.IsNewVersion = false;
        }
        else
         appLatestVersionSettings.IsNewVersion = false;
       }
      }
     }
     catch (Exception ex)
     {
      _loggingService.LogErrorFormat(ex, MethodBase.GetCurrentMethod().Name);
     }
     return appLatestVersionSettings;
    }
```

**Notes:**  
  
* The method return type has been changed from Task to a Strongly-Typed Task
* The method returns a value. Before the refactor, the method only initialized a class-scoped variable
* The method now has a try/catch block and therefore can report more accurate exception information ie MethodBase.GetCurrentMethod().Name
* The method returns a null object if an exception is thrown

### MainForm.cs

```csharp
    private async Task CheckForNewVersion()
    {
     IAppNotificationService appNotificationservice = new AppNotificationService(Program.GetApplicationTag(), _loggingService);
    
     var task = await appNotificationservice.GetLatestVersionInfo();
    
     if (task != null)
      this.CheckForNewVersionCompleted(task);
     else
      _displayErrorMessageService.DisplayError(new ErrorMessage() { DisplayMessage = "An error occurred while trying to check for a new verison", DisplayTitle = "Error Occurred." });
    }
```

**Notes:**  
  
* The method now has an async keyword in the method declaration
* The method specifies a Task return type
* The method uses the _await_ keyword
* The method checks to see if the returned AppNotificationService task is null
* If the AppNotificationService task is null, the service that displays errors to the user is called
* If the AppNotificationService task is not null, the task is passed to a new function that has the code that was in the Backgroundworker RunWorkerCompleted Event
* After _await_, the method continues running on the UI thread. The CheckForNewVersionCompleted method updates UI controls and you don't receive cross-thread exceptions.
* The Backgroundworker component has been removed completely

```csharp
    private void CheckForNewVersionCompleted(AppLatestVersionInfo result)
    {
     _appLatestVersionInfo = result;
     if (_appLatestVersionInfo != null && _appLatestVersionInfo.IsNewVersion)
     {
      pbNewVersion.Visible = true;
      this.ConfigurepbNewVersion();
     }
    }
```

**Notes:**  
  
* The new method has the code that was in the Backgroundworker RunWorkerCompleted Event

Final Notes
------------

After refactoring my open source software code to use async/await, I feel that the code is more intuitive, consistent and a lot easier to read for someone not familiar with the codebase. The changes discussed in this blog post are in a feature branch and have not been released because the Backgroundworker is being used in several other places throughout the codebase.  
  
After writing this blog post, one item that I am thinking about modifying is the CheckForNewVersion method in MainForm.cs. I am not sure that checking for a null task is the best way to determine if the asynchronous call threw an exception. One idea might be to add a new custom exception property to the AppLatestVersionInfo object, hydrate it with any exceptions thrown in the AppNotificationService and then check this property in CheckForNewVersion.  
  
One of the reasons I like to blog is because it forces me to take the time to try to really understand concepts and this helps me to become a much better technologist. Also, I would like to give credit to [Stephen Cleary](http://blog.stephencleary.com/). Stephen has a wonderful blog that has posts that goes into great detail about the inner workings of how async/await.
