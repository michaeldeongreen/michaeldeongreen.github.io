---
layout: post
title:  "Implement async/wait in the Angular 2 Tour of Heroes Web API"
date:  2016-11-30
categories: [technology]
images: michaeldeongreen-logo.png
tags: [asp.net, web api, async-wait]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

Outside of calling a Web API, I have rarely used the async/wait functionality in the .Net Framework explicitly. I spent some time this week really digging into the concepts of [async/wait](https://msdn.microsoft.com/en-us/library/mt674882.aspx) because I would like to take advantage of asynchronous programming in the future. I decided to implement async/wait in the Heroes Search Endpoint in the Angular 2 Tour of Heroes Web API I have hosted in Azure. I wanted to create a very brief blog outlining the changes I made.  
  
You can find the Angular 2 Tour of Heroes Web API and a link to the GitHub repository [here](http://angularioquickstartwebapi.azurewebsites.net/).  
  
Here is an overview of my project structure:  
  
[![michael-d-green-grenitausconsulting-com-making-the-angular-2-tour-of-heroes-web-api-heroes-search-in-azure-asynchronous-001](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-making-the-angular-2-tour-of-heroes-Web-api-heroes-search-in-azure-asynchronous-001.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-making-the-angular-2-tour-of-heroes-Web-api-heroes-search-in-azure-asynchronous-001.png)  
  
Web API Logical Architecture
----------------------------

* **Angular.io.QuickStart.Web.Api** - Web Service layer that contains the ASP.Net MVC Controllers that clients will call.
* **Angular.io.QuickStart.Web.Api.Models** - Business Domain layer that contains all the domain objects used in the Web API.
* **Angular.io.QuickStart.Web.Api.Repository** - Data Access layer that uses Entity Framework, the repository and unit of work patterns to access data stored in SQL Azure.
* **Angular.io.QuickStart.Web.Api.Services** - Business layer that sits in between the Web Service and Data Access layers. It is primarily responsible for validation and returning data from the Data Access layer to the Web Service layer.

Synchronous Code Snippet
------------------------

Here is a brief code snippet of the code before I made the Hero Search Asynchronous:  
  
**In the HeroesSearchController _(Angular.io.QuickStart.Web.Api)_:**

```csharp
            /// 
            /// Endpoint is used to get all heroes whose name contains the name parameter
            /// 
            /// 
            /// List of heroes whose name contains the name parameter
            public IEnumerable Get(string name)
            {
                return  _heroService.Find(name);
            }
```

**In the HeroService_(Angular.io.QuickStart.Web.Api.Services)_:**  

```csharp
            /// 
            /// Endpoint is used to get all heroes whose name contains the name parameter
            /// 
            /// 
            /// List of heroes whose name contains the name parameter
            public IEnumerable Get(string name)
            {
                return  _heroService.Find(name);
            }
```

**In the Generic Repository_(Angular.io.QuickStart.Web.Api.Repository)_:**

```csharp
            public Repository(TourOfHeroesContext dbContext)
            {
                _dbContext = dbContext;
                _dbSet = dbContext.Set();
            }
    
            public virtual IEnumerable Find(Expression> filter)
            {
                IQueryable query = _dbSet;
    
                if (filter != null)
                {
                    query = _dbSet.Where(filter);
                }
    
                return query.ToList();
            }
```

Asynchronous Code Snippet
-------------------------

To implement async/wait on the Heroes Search Endpoint, I started with the Data Access later and worked upwards. Below are the small changes I made:  
  
**Added new async method called FindAsync to the Generic Repository_(Angular.io.QuickStart.Web.Api.Repository)_:**  

  ```csharp
            public virtual async Task> FindAsync(Expression> filter)
            {
                IQueryable query = _dbSet;
    
                if (filter != null)
                {
                    query = _dbSet.Where(filter);
                }
    
                return await query.ToListAsync();
            }
```

**Added a new async method called FindAsync to the HeroService_(Angular.io.QuickStart.Web.Api.Services)_:**

```csharp
            public async Task> FindAsync(string name)
            {
                _heroValidationService.SearchValidation(new HeroDTO() { Name = name });
    
                return await _uow.HeroRepository.FindAsync((x) => x.Name.ToLower().Contains(name.ToLower()));
            }
```

**Modified the existing Get Endpoint on the HeroesSearchController to be async_(Angular.io.QuickStart.Web.Api.)_:**

```csharp
            /// 
            /// Endpoint is used to get all heroes whose name contains the name parameter
            /// 
            /// 
            /// List of heroes whose name contains the name parameter
            public async Task> Get(string name)
            {
                return await _heroService.FindAsync(name);
            }
```
