---
layout: post
title:  "How to Implement StructureMap 4 in a WCF Service using Nested Containers"
date:  2018-02-10
categories: [technology]
images: michaeldeongreen-logo.png
tags: [StructureMap, WCF, Legacy Code, Dependency Injection]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

I recently created a proof of concept GitHub repository and GitHub Gist on how to implement StructureMap 4 in a WCF Service using Nested Containers for a client who deals with lots of legacy code. A good portion of the code came from [Jimmy Bogard's blog](https://lostechies.com/jimmybogard/2008/07/30/integrating-structuremap-with-wcf/) and he does a great job of explaining the purpose of each class.  
  
I built upon his example to use the latest version of StructureMap, which is currently Version 4 and implemented Nested Containers per Operation Request. The GitHub Repository should be fairly straightforward to follow and I have also created a GitHub Gist to point out the bare minimum classes that I used to accomplish this task.  
  
The GitHub Repository can be found here:  
[https://github.com/michaeldeongreen/GreenWCF](https://github.com/michaeldeongreen/GreenWCF)  
  
The GitHub Gist can be found here:  
[https://gist.github.com/michaeldeongreen/59c01f9b7fe45636b1aa5f946ac0adfb](https://gist.github.com/michaeldeongreen/59c01f9b7fe45636b1aa5f946ac0adfb)
