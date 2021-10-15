---
layout: post
title:  "Angular 2 Tour of Heroes Web API in Azure"
date:  2016-11-20
categories: [technology]
images: azure-angular-web-api.png
tags: [angular 2, asp.net web api, azure, azure api app, azure web app, c#, entity framework, sql server, structuremap, swagger]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/azure-angular-web-api.png)

The Angular 2 Tour of Heroes guide created by the Angular Team is a wonderful introduction to Angular 2. For those who have completed the guide, you already know that the last step, HTTP, creates an angular-in-memory-web-api to demonstrate how Angular 2 works with REST services.  
  
After I completed the Angular 2 Tour of Heroes guide, I decided to create a Heroes ASP.Net Web API and host it in the Azure cloud for others to use. I wanted to see how easy it would be to swap out the angular-in-memory-web-api and to see how Angular 2 works with a real REST API.  
  
**Angular 2 Tour of Heroes Web API Technology Stack**  
  
* ASP.Net Web API 2
* Azure Web App
* CORS enabled for all schemes, domains and ports
* C# 6
* Entity Framework 6
* SQL Server 2014 (local)/SQL Azure (deployed)
* StructureMap 3
* Swashbuckle/Swagger
* IoC, Repository, Service Locator and Unit of Work Pattern

**Resources**  
  
* **Angular 2 Tour of Heroes Guide** - You can find the official guide on the Angular 2 website [Here](https://angular.io/docs/ts/latest/tutorial/)
* **My Angular 2 Tour of Heroes Application** - I created a GitHub repository for my version of the Tour of Heroes guide and it has been deployed to Azure. This version of the Tour of Heroes guide has been modified to connect to the ASP.Net Web API I created. You can find the application [here](http://angulariotourofheroesweb.azurewebsites.net/) and the GitHub repository [here](https://github.com/michaeldeongreen/Angular.io.QuickStart.Web)
* **My Angular 2 Tour of Heroes REST API** - You can find the Angular 2 Tour of Heroes REST API I created and deployed to Azure [here](http://angulariotourofheroeswebapi.azurewebsites.net/) and the GitHub repository [here](https://github.com/michaeldeongreen/Angular.io.QuickStart.Web.Api)

**Step By Step Changes**  
  
This portion of the blog is for those who completed the Angular 2 Tour of Heroes guide and would like a walk through of the changes that need to be made to allow the application to connect to the ASP.Net Web API I have created and is hosted in Azure.  
  
**Step 1 - app.module.ts**  
  
First, we need to remove the import statements for the angular-in-memory-web-api and InMemoryDataService. Open app.module.ts and remove the following lines:  
  
```typescript
    
    // Imports for loading & configuring the in-memory web api
    import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
    import { InMemoryDataService }  from './in-memory-data.service';
    
    InMemoryWebApiModule.forRoot(InMemoryDataService),
``    

  
  
**Step 2 - hero.service.ts**  
  
Next, we need to modify the base url and modify the json response notation in hero.service.ts  
  
Find the line below:  
  
```typescript    
    private heroesUrl = 'app/heroes';  // URL to web api
```
  
And modify it to:  
  
```typescript
    
    private heroesUrl = 'http://angulariotourofheroeswebapi.azurewebsites.net/api/heroes';
    
```
  
In the getHeroes method, find the line:  
  
```typescript
    
    .then(response => response.json().data as Hero[])
    
```
  
And modify it to:  
  
```typescript
    .then(response => response.json() as Hero[])
    
```
  
In the create method, find the line:

```typescript
    .then(res => res.json().data)
```

And modify it to:  

```typescript  
   
    .then(res => res.json())
```

**Step 3 - hero-search.service.ts**  
  
Next, we need to make a small changes in hero-search.service.ts.  
  
Change the entire HeroSearchService class to:

```typescript
    export class HeroSearchService {
        constructor(private http: Http) { }
        search(term: string): Observable {
            return this.http
                .get(`http://angulariotourofheroeswebapi.azurewebsites.net/api/heroessearch?name=${term}`)
                .map((r: Response) => r.json() as Hero[]);
        }
    }
```

**Notes**  
  
* I created a separate Web API controller for HeroesSearch because currently Swagger seems to have issues with multiple overloaded action methods ie Get(int id), Get(string name) etc etc
* The Angular 2 Web API uses SQL Server 2014 locally. So if you decide to clone the repository, ensure that you have SQL Server installed or change the connection string to match the datastore in your environment
