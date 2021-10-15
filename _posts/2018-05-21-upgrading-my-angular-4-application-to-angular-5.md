---
layout: post
title:  "Upgrading My Angular 4 Application to Angular 5"
date:  2018-05-21
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Angular 2, TypeScript, NPM]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

I just wanted to write a very short blog entry on the steps I took to upgrade my blog from Angular 4 to Angular 5.  
  
**Package Updates**  
  
The very first steps I took were to create a new branch in GitHub and also, I used a Virtual Machine to perform the upgrade before performing all the steps on my main PC.  
  
* I upgraded Node.js and Node Package Manager (npm) to the latest version
* I used npm to upgrade angular-cli to the latest version
* I used npm to upgrade Typescript to the latest version
* I downloaded and installed the latest Typescript SDK for Visual Studio 2017
* I configured Visual Studio 2017 to use the latest version of Typescript by selecting the Class Library project where the Angular files reside, right-clicking and selecting _properties_ and navigating to the _TypeScript Build_ tab:
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/grenitausconsulting-upgrading-my-angular-4-application-to-angular-5-001.png)*   I used [https://update.angular.io/](https://update.angular.io/) to help me identify which node modules needed to be upgraded, the exact versions and generate the npm commands. **Note:** Make sure to delete the _node\_modules_ folder before running the npm install command

* I then used the command _ng update @angular/cli_ to create the new angular.json configuration file
* I had to replace the now deprecated http service with the new httpclient. I will talk about this in more detail later but [here](https://medium.com/codingthesmartway-com-blog/angular-4-3-httpclient-accessing-rest-web-services-with-angular-2305b8fd654b) is a great blog I read to help me convert the http service to the new http client.

**Observations**  
  
**[.angular-cli.json](https://github.com/michaeldeongreen/Blog.Michaeldeongreen/blob/6413886cda1f2d0d9646a90e9fe3907b853e34c8/Client/.angular-cli.json)** - This file has been deprecated and a new file called [angular.json](https://github.com/michaeldeongreen/Blog.Michaeldeongreen/blob/master/Client/angular.json) is now used.  
  
**http service** - The http service has been deprecated in place of the new HttpClient.  
  
Server Side:  
  
```csharp
    
    [Route("api/search/{criteria}/page/{pagenumber}")]
    public PagedResponse Get(string criteria, int pageNumber)
    {
     return _pagingService.Search(new PagedCriteria() { PageNumber = pageNumber, PageSize = _pageSize, Posts = BlogContextManager.PostSummaries, SearchCriteria = criteria, IsActive = true });
    }
    
```
  
Using the http service:  
  
```typescript
    
    import { Http, Response, Headers, RequestOptions } from '@angular/http';
    import { Observable } from 'rxjs/Rx';
    import 'rxjs/add/operator/map';
    import 'rxjs/add/operator/catch';
    
        //...
        constructor(private http: Http) {
            //...
        }
    
        public getSearchResults(page: Number, criteria: string): any {
            let url = `${this.baseUrl}/search/${encodeURIComponent(criteria)}/page/${page}`;
            return this.http.get(url)
                .map((response: Response) => response.json())
                .catch((error: any) => Observable.throw(error.json().error) || 'Server Error');
        }
    
```

Using the new http client:  
  
```typescript
    
    
    import { HttpClient } from '@angular/common/http';
    import { IPagedResponse } from './ipaged-response.pagedresponse';
    
        //....
        constructor(private http: HttpClient) {
            //...
        }
    
        public getSearchResults(page: Number, criteria: string): any {
            let url = `${this.baseUrl}/search/${encodeURIComponent(criteria)}/page/${page}`;
            this.http.get<IPagedResponse>(url)
        }
        
```

That's pretty much it. In a few days, I will probably upgrade from Angular 5 to Angular 6.
