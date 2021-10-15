---
layout: post
title:  "Another Reason Why I Love Unit Tests Part 2!"
date:  2018-02-26
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Test Driven Development, Unit Testing]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

A few years ago, I wrote a blog entry about [why I love Unit Tests](https://blog.michaeldeongreen.com/technology/2015/06/11/another-reason-why-i-love-unit-tests.html). Recently, I discovered another reason why I love Unit Tests and I just wanted to write a short blog entry about the joys of Unit Testing  
  
**The Project!**  
  
My client's application is a legacy appliacation that consists of pulling, parsing and storing Credit Report Information. Like many legacy applications, when it was written years ago, Unit Testing was not taken into consideration and as a result, as the application grew, it made it increasingly harder to test. Since I joined the project, I have really preached the need to re-factor the codebase to make the application more Unit Testable and we, as a team have started to take this advice to heart.  
  
Prior to our re-factoring effort, to test certain pieces of code, you would need to run several different instances of Visual Studio locally, just so you could put a breakpoint in our application to test any changes. In certain situations, you could also use a Test Harness that the team built out years ago to make testing easier.  
  
Regardless, to test the application required considerable effort. Also, you had to pray that at the time you were doing the testing that the other Web Services you needed to run locally, that had nothing to do with the changes you wanted to test were working properly because they themselves were being changed by other developers.  
  
**What Happened!**  
  
Recently, some really crucial business logic was changed in this application. Due to our efforts to make the codebase more Unit Testable, the person who made the change really took the [The Boy Scout Rule](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule) to heart, re-factored this area and wrote about 124 brand spanking new Unit Tests.  
  
**The Epiphany!**  
  
I happened to draw the short straw and had to do the Code Review for this change. As I mentioned earlier, testing this part of the application would take some effort and would normally take a signficant amount of time. However, because this person took the time to re-factor the codebase, isolate the business logic and make it more Unit Testable, I was able to do a few things to make life easer:  
  
* Review her 124 Unit Tests to better understand the business logic
* Create temporary Unit Tests with my own mock data to target certain scenarios
* Avoid having to basically do Integration Tests, just to test the logic in Real Time
* Literally cut the Test portion of the Code Review by maybe 60%-70%
* Last but not least, I could now target the exact code that was changed and only that code
