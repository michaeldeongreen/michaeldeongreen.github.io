---
layout: post
title:  "My First Impressions of Angular 2!"
date:  2017-02-15
categories: [technology]
images: michaeldeongreen-logo.png
tags: [angular 2, angularjs, javascript, json, node.js, npm, rest, systemjs, typescript, visual studio, asp.net web api, webpack]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In this blog post, I wanted to discuss my first impressions of Angular 2. I want to discuss why I decided to take a look at Angular 2, some of the issues that I faced when trying to learn the framework, my overall opinion of Angular 2 and finally, provide links to some resources that I found useful. If you would like an in depth tutorial on Angular 2, you can visit the official website [here](https://angular.io/).  
  
**Background!**  
  
First, just a little background on me. I have several years of JavaScript experience but I didn't start working with modern JavaScript frameworks until around 2013. The first JavaScript framework that I worked with at a client was [KnockoutJS](http://knockoutjs.com/documentation/introduction.html) and after that, I worked on my first Angular 1 project for a little over a year. I don't consider myself an expert on Front-End development but I believe I have enough experience to make an informative blog about Angular 2.  
  
Last year I started to become interested in Angular 2 because one of my clients was interested in creating a new Self-Service Portal and Angular 2 was being viewed as an option. As I started to learn about Angular 2, I wrote a few blogs about my experiences, which you can find [here](http://Blog.Michaeldeongreen.com/?s=angular+2).  
  
**The Learning Curve!**  
  
As a .Net developer, when you start trying to learn about Angular 2, you will ultimately find your way to the [Official Visual Studio Quickstart Guide](https://angular.io/docs/ts/latest/cookbook/visual-studio-2015.html). When confronted with this guide, my first thought was that the Angular 2 learning curve was pretty high. I had to become familiar with concepts such as [polyfills](https://en.wikipedia.org/wiki/Polyfill), [transpiling](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them), [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) and [JavaScript Module Loaders](https://appendto.com/2016/06/the-short-history-of-javascript-module-loaders/). I already had a basic working knowledge of [NPM](https://www.npmjs.com/) but I had to learn about config files such as [package.json](https://docs.npmjs.com/files/package.json) and [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html). I had to become familiar with [SystemJs](https://github.com/systemjs/systemjs), [TypeScript](https://www.typescriptlang.org/) and to a lesser extent, [RXJS](http://reactivex.io/rxjs/).  
  
Have you noticed that despite all the things I had to become familiar with, I still haven't mentioned the actual Angular 2 Framework? The learning curve for Angular 2 is high mainly because of its ecosystem. Angular 2's ecosystem presents a significant barrier to entry to learning the new version of the framework but once you become familiar with the ecosystem, you can finally start to learn the actual framework.  
  
**Visual Studio**  
  
I had a conversation with a co-worker last year about Visual Studio and JavaScript frameworks. I told him that I felt that once modern JavaScript frameworks like React and Angular started to see mass adoption, that Visual Studio started to "get" in the way. As primarily a .Net developer, when I start to learn a new JavaScript framework, the first thought that comes mind is "How do I set this framework up in a Visual Studio Project?". When I started trying to learn Angular 2, most of the examples were targeted towards developers familiar with the "Open Source" ecosystem, chiefly [Node.js](https://nodejs.org/en/) and [Gulp](http://gulpjs.com/). As mentioned earlier, the Angular 2 team tried to solve this problem by creating a specific tutorial for how to get an Angular 2 project up and running with Visual Studio, which was of great use to me.  
  
**TypeScript**  
  
As I continued down my path of learning Angular 2, I came across another non-trivial roadblock, TypeScript. TypeScript is a JavaScript superset that brings Type Safety to JavaScript. With the rise of JavaScript frameworks and the increase complexity of Front-End development, it is not surprising that a technology such as TypeScript would arrive on the scene sooner or later. When I first started working on the Visual Studio Quickstart Guide, I had hundreds of of TypeScript compiler errors in Visual Studio. Although I installed the latest version of TypeScript globally via the Node Package Manager, I could not get the Quickstart guide to compile.  
  
After reading countless blogs and articles, I finally found out that it doesn't matter what version of TypeScript you have installed globally via the Node Package Manager or that you have instructed Visual Studio to evaluate certain paths first to determine which 3rd party tools to use, like the image ![michael-d-green-grenitausconsulting-com-my-first-impressions-of-angular-2-001.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-my-first-impressions-of-angular-2-001.png), Visual Studio is still going to use the TypeScript version in the path below:  

```typescript  
    C:\Program Files (86)\Microsoft SDKs\TypeScript
```

Once I understood this fact and upgraded my version of TypeScript that Visual Studio uses via the official [TypeScript for Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=48593) install, I was finally able to compile the Visual Studio Quickstart Guide.  
  
I won't go into the ins and outs of TypeScript but it does represent yet another hurdle to learn Angular 2. You can learn more about TypeScript [here](https://www.typescriptlang.org/).  
  
**Boilerplate!**  
  
As I alluded to earlier, there is a lot of boilerplate code you have to setup to get started with Angular 2. In fact, I believe the Angular 2 Team started to realize this, so they created a tool called [angular-cli](https://github.com/angular/angular-cli) to help. As of this blog post, angular-cli is still in beta and as I have stated, this is yet another tool that a developer may have to learn to get started with the Angular 2 framework. Also, it is my understanding that angular-cli was recently modified to replace SystemJS with [Webpack](https://webpack.github.io/docs/) as the default Module Loader.  
  
**Angular 2!**  
  
Once you get past learning the new ecosystem, base concepts, setting up Visual Studio and TypeScript, then you can focus on the actual framework itself. As someone who has developed in Angular 1, I can safely state that just because you know Angular 1 or AngularJS, this doesn't mean that you know Angular 2. I had to learn how properly configure TypeScript via the tsconfig.json, how to setup dependencies via the package.json, how to bootstrap the Angular 2 application, Angular 2 naming conventions, "everything is a component" paradigm and the new Angular 2 Syntax.  
  
As I worked through the Visual Studio Quickstart guide and eventually, the [Tour of Heroes guide](https://angular.io/docs/ts/latest/tutorial/), I felt that very few of my Angular 1 skills were transferable and that I was literally learning an entirely new framework. In fact, Angular 2 is so different from its predecessor, that many times I jokingly thought to myself that the Angular 2 Team may have been better off calling Angular 2 something completely different.  
  
After I completed the Tour of Heroes Guide, I ended up writing a blog on how to host it in Azure which you can find [here](post/angular-2-continuous-deployment-azure-azure-cli-github/). Also, the Tour of Heroes Guide uses a mock REST service and I wrote another blog post where I outline creating a Web API REST service, hosting it in Azure and modifying the Tour of Heroes application to use it, which you can find [here](post/angular-2-tour-of-heroes-web-api-in-azure/).  
  
**Conclusion!**  
  
Overall, it has been a very interesting, frustrating but fun learning experience to try to learn Angular 2. I feel that the learning curve is extremely high and because of this, it may very well force some to evaluate other JavaScript frameworks such as [React](https://facebook.github.io/react/tutorial/tutorial.html). To be fair, I am not very familiar with other frameworks such as React, so I don't have a complete understanding of whether or not it is equally challenging to learn those frameworks.  
  
One exciting aspect of learning Angular 2 is that the skills that you learn are very transferable to other frameworks and technology stacks. Node, NPM and JavaScript Module Loaders are very in demand skills and you will definitely start to learn some of their nuances, once you get a deeper understanding of Angular 2.  
  
As for the project for my client where I thought about using Angular 2, we ended up just re-skinning the existing website. I know that my former client recently wrote a new application in Angular 2 but I don't know how complicated the application is and what problems their development team faced.  
  
As of now, I would be very hesitant to put an Angular 2 application into production. I feel that the ecosystem and tools are still not mature enough and there aren't a lot of tutorials and samples available to guide you. However, I am sure someone that is more adept at front-end development could make the case that Angular 2 is production ready. Ultimately, I think whether or not to implement a new project in Angular 2 depends on what type of project you have, the skill set of your development team and how much time you have to learn the new framework.  
  
**Helpful Resources!**  
  
Below are some the resources I found helpful when I started trying to learn Angular 2.  
  
[A great article about the history of JavaScript and JavaScript versioning](https://benmccormick.org/2015/09/14/es5-es6-es2016-es-next-whats-going-on-with-javascript-versioning/)  
  
[Official TypeScript Website](http://www.typescriptlang.org/)  
  
[Official Angular 2 Website](https://angular.io/)  
  
[Great article about the history of JavaScript Module Loaders](https://appendto.com/2016/06/the-short-history-of-javascript-module-loaders/)  
  
[Great article that talks about what version of TypeScript Visual Studio uses](http://www.allenconway.net/2015/07/which-version-of-typescript-is.html)  
  
[Official Angular 2 Quickstart Guide](https://angular.io/docs/ts/latest/quickstart.html)  
  
[Angular 2 Tour of Heroes Tutorial](https://angular.io/docs/ts/latest/tutorial/)
