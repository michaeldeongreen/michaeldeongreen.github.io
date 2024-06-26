---
layout: post
title:  "Replacing My Wordpress Blog with Angular 2 and ASP.Net Web API Part 2"
date:  2017-04-04
categories: [technology]
images: replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api.png
tags: [ASP.Net Web API, Angular 2, NUnit, CSS, TypeScript, Visual Studio 2015, C#, StructureMap, Json.net, Bootstrap 3, SEO, angular-cli, prismjs]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api.png)

In part 2 of this blog series about replacing my wordpress blog with Angular 2 and ASP.Net Web API, I wanted to go into more detail about the Angular 2 Front-end application. If you missed part 1 of this blog series you can find it [here](https://blog.michaeldeongreen.com/technology/2017/03/28/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-1.html). In part 1, I stated that there were 3 basic components that I implemented to complete the project. I built a Front-end, written in Angular 2, a Back-end, using ASP.Net Web API and a Command Line Interface tool (cli), which is a Windows Console Application. You can view a diagram of the 3 components [here.  
  
**angular-cli**  
  
[The first task I completed was to generate a template using the powerful](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-1-002.png) [angular-cli tool.](https://github.com/angular/angular-cli) In the [blog post](post/upgrading-my-homepage-to-angular-2-using-angular-cli) where I wrote about re-writing my homepage in Angular 2, I talked about how angular-cli makes getting started with, development and deploying Angular 2 applications very easy. I used angular-cli to create my starting Angular 2 folder structure, view changes to my application in real time and to create distribution artifacts. Visual Studio 2015 was used to ensure TypeScript would compile and for creating the Web API and Console Application.  
  
**The Angular 2 Application**  
  
Due to time constraints and the fact that my CSS and overall Web Design skills are not the best, I needed to find a template for my new blog in pure HTML5, so I could customize it. I eventually settled on startbootstrap's blog home template, which you can find [here](https://startbootstrap.com/template-overviews/blog-home/). The template uses Bootstrap 3 and is very easy to break into smaller, Angular 2 components. The diagram below denotes how I decided to break the template up into Angular 2 components:  
  
![replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-2-001.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-2-001.png)  
  
**Sibling Component Communication**  
  
One of the issues that I faced immediately was I didn't know how to get sibling components in Angular 2 to talk to each other. I created a component called "search-results" that would be responsible for calling my Web API and displaying the results. What I noticed is that after using routing the first time for the search-result component, its ngOnInit method would only be called one time, on first creation of the component. So I needed a way to call the search-result component on subsequent searches.  
  
After doing some research, I ended up reading about the concept of "Shared Services" in Angular 2. The idea is to create a service that can be injected into components and used as a means for components to communicate with each other. Here is an overview of what I did:  
  
I created a service called "shared-emitter.service":  

```typescript  
    import { Injectable, EventEmitter } from '@angular/core';
    
    @Injectable()
    export class SharedEmitterService {
    
        searchCriteriaChanged = new EventEmitter();
    
        constructor() { }
    
        searchCriteriaChangedEvent(value) {
            this.searchCriteriaChanged.emit(value);
        }
    }
```

In this service, I have made it injectable, I declared an EventEmitter and a method that allows components to pass a value into the service and "emit" it to components that are interested in the event.  
  
Next, in my app.module, I imported the shared-emitter service and place it in the list of global providers:  

```typescript  
    import { SharedEmitterService } from './shared-emitter.service';
    
    @NgModule ({
    ....
      providers: [SharedEmitterService]
    })
```

In my search.component.ts file, I did following:  
  
```typescript  
    import { SharedEmitterService } from '../shared-emitter.service';
    
    export class SearchComponent implements OnInit {
        constructor(public sharedEmitterService: SharedEmitterService) { }
     
    gotoSearchResults(textBox: HTMLInputElement): void {
          this.sharedEmitterService.searchCriteriaChangedEvent(textBox.value);
          let link = ['/searchResults', textBox.value]
          textBox.value = null;
          this.router.navigate(link);
      } 
```

I have injected the shared-emitter service into the search component and have a method that will accept an html element, fired the "searchCriteriaChangedEvent" event and routes to the search-results component. The first time the user does a search, the ngOnInit method of search-results will fire but will not on any other searches, unless the user refreshes the entire application.  
  
In my search.component.html file, I did the following:  
  
```html
    <input #searchBox type="text" class="form-control" id="search-box" (keyup.enter)="gotoSearchResults(searchBox)">
```

When the user enters search criteria and hits enter, the "gotoSearchResults" method will be called and the input control will be passed as a parameter. In the search-results component, I have done the following:  
  
```typescript  
    import { SharedEmitterService } from '../shared-emitter.service';
    
        constructor(private sharedEmitterService: SharedEmitterService) {
    
            this.sharedEmitterService.searchCriteriaChanged.subscribe(criteria => {
            });
        }
```

In the search-results component, I have injected the shared-emitter service into it and I have subscribed to the searchCriteriaChanged event. Each time this event is fired, the code inside the "arrow" method will be called and this is how I am able to display search results after the user has performed a search for the first time.  
  
**Angular 2 Paging**  
  
The next order of business was to find an existing library that would do pagination for me. When I did my first google search, I came across Michael Bromley's [ng2-pagination](https://github.com/michaelbromley/ng2-pagination). I was able to get the basic example to work but to my great frustration, I couldn't get the server paging to work. I spent a lot of time trying to get it to work and at this time, I don't even remember what errors it gave me or what exactly was wrong, I just remember I couldn't get it to work.  
  
After spending hours trying to get this solution to work, I decided to look at other options and came across Jason Watmore's [blog](http://jasonwatmore.com/post/2016/08/23/angular-2-pagination-example-with-logic-like-google) about how to do pagination in Angular 2 and was able to use his solution. The only issue that I had is that he uses underscore.js and I couldn't get the PagerService to compile. After doing some research, I found out I needed to install the "@types/underscore" library for TypeScript.  
  
**Prismjs**  
  
Prismjs worked wonderfully out of the box as a syntax highlighter but I ran across a few issues where code snippets would be empty and though this isn't an Angular 2 problem, it was something that had me stumped for a little while. As far as I can tell, I think if prismjs doesn't recognize the syntax of the language that you have specified via "class=language-somelanguage", it just elects to hide the entire code block. After doing some research, the solution was to wrap the code around the tags to prevent prismjs from hiding the code block.  
  
**Social Media Share Buttons** Another feature I needed to implement was social media share buttons. After doing a quick google search, I came across [this](https://simplesharebuttons.com/html-share-buttons/) great tutorial on how to set up share buttons and where to get the social media icons for sites like Facebook, Google+ and LinkedIn. The hardest part was figuring out how to get the scheme and domain in Angular 2 and I eventually used the following to get them:  
  
```typescript  
    window.location.protocol //scheme
    
    window.location.host //domain
```

**404 errors!**  
  
The next issue that I faced was when I would deploy the application to Godaddy's application servers, whenever I would refresh the page or hit the back button, I would get "404 resource not found" errors. I had faced this issue when I re-wrote my homepage in Angular 2 and I just used the [HashLocationStrategy](https://angular.io/docs/ts/latest/api/common/index/HashLocationStrategy-class.html) to fix the issue but the price you pay is that it adds the hashtag to your url. So once again, doing some google searches brought me to a [solution](https://github.com/angular-ui/ui-router/wiki/Frequently-Asked-Questions#how-to-configure-your-server-to-work-with-html5mode) which uses re-write rules in the Web.config file. In my Web.config, I did the following:  

```xml  
      <system.webServer>
        <rewrite>
          <rules>
            <rule name="Main Rule" stopProcessing="true">
              <match url=".*" />
              <conditions logicalGrouping="MatchAll">
                <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
                <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
              </conditions>
              <action type="Rewrite" url="/" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>  
```

When I would first navigate to my blog, everything seemed to be working. But after a few requests, I started getting these weird and cryptic "e.json" errors. After spending hours pulling my hair out trying to figure out what the problem was, I ultimately went back to hashtags. But alas, fate smiled on me as I was talking to a friend about the issue and he asked me was the Web API and Angular 2 site hosted together. Not sure why I hadn't thought of this but when I would try to hit a Web API Endpoint, I would get routing errors, as if Angular 2 was taking over routing for the Web API.  
  
So what I did is create a sub domain on Godaddy called api.grenitausconsulting.com and deployed the Web API there. I re-deployed the Angular 2 site by itself, with the Web.Config re-write rules and everything worked. What I believe was happening was that the "/api" calls were being blocked somehow and since REST calls were being blocked or negated, it caused problems with the Angular 2 framework. My friend sent me a modified version of the Web.Config re-write rules which he felt would exclude "/api" calls but I had already grown comfortable with the Web API and Angular 2 client being deployed separately.  
  
**In Need of a Good Spinner!**  
  
Godaddy seems to have some very aggressive IIS app pool settings. I am on a shard plan so I cannot access certain settings but I suspect that they have set the app pool sleep interval very low and this is why it may take a little longer for the site to load if you haven't visited the site in several minutes (later I will talk about how I am trying to get around this). The result of this is that the Angular 2 application would be empty and you wouldn't know if there was an error or if the REST call hadn't completed. So after doing yet more research, I came across [this](https://www.w3schools.com/howto/howto_css_loader.asp) handy CSS spinner that I modified for my own blog. I could have used Fontawesome, I just didn't want to use another library just to get the spinner functionality.  
  
**Misc.**  
  
I ran into several miscellaneous issues while working on the Front-end. If you use Godaddy, make sure you uninstall [Roslyn](https://blog.gldraphael.com/removing-roslyn-from-asp-net-4-5-2-project-template/) because Godaddy blocks it and you will get errors when you attempt to run your application.  
  
Also, the Angular 2 syntax does take some time to get used too ie "\*ngIf" or \[someAttribute\]="someVariable" but I think once I started working with Angular 2 a lot more, I got use to the syntax. Also, I really love the back-tick and string concatenation with "${}".  
  
Another odd quirk that I found with Angular 2 is that sometimes when you have an error, the only way it will manifest itself is by firing a components' "ngOnInit" event twice. On several occasions I was stuck trying to figure out why something wasn't working and one of the byproducts was ngOnInit firing twice and there is even a person that posted I think on GitHub or StackOverflow about "How it was ridiculous for the ngOnInit shouldn't fire twice just because there is an error".  
  
And the last item is that I noticed that Angular 2 is very "noisy" and broadcast itself all over the HTML markup. Whenever I would look at the source via the Developer Tools in the browser, I would see tons of places in the markup where Angular 2 had broadcasted itself.  
  
Which reminds me, I thought it was pretty cool that you could debug TypeScript files in the developer tools. I didn't know this until I started this project. For those who didn't know this, see the image below:  
  
![replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-2-002.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-2-002.png)  
  
**Conclusion**  
  
This wraps up another blog post for me. I hope I was able to provide some decent insight on how to get started with Angular 2. In the next part of this series, I will talk about the ASP.Net Web API and the cli tool I created to help me complete the project.
