---
layout: post
title:  "Another Reason Why I Love Unit Tests!"
date:  2015-06-11
categories: [technology]
images: michaeldeongreen-logo.png
tags: [test driven development, unit testing]
---

## The Project
  
On a recent project I worked on, our team made a strong commitment in the last 3 Sprint iterations to write more Unit Tests. The commitment to write more Unit Tests resulted in members of our team thinking “Test” first and writing software that was easier to test, to be modified and maintained.  
  
## What Happened?
  
Over a recent 2-3 day stretch I wrote about 40 Unit Tests as I refactored quite a bit of code because in the previous Sprint, I had to rush through implementing certain requirements due to the lack of time. We used [TeamCity](https://www.jetbrains.com/teamcity/ "Team City") for [Continuous Integration](http://www.thoughtworks.com/continuous-integration "Continuous Integration") on this particular project and whenever we checked-in code, Unit Tests would run and it would tell us the number of Unit Tests that ran successfully.  
  
One day, I arrived in the office and a few bizarre things happened. First, when I pulled the latest code from Git, I was told there were merge conflicts. I knew something wasn’t right because I always commit and push my changes before I leave everyday. Also, the files that were in conflict were only files I had solely worked on. Second, when I looked at TeamCity, I saw that our Unit Test count had been drastically reduced.  
  
Upon further investigation, I found that another developer had mistakenly overwritten all of my changes over the last few days. I later found out that a Git rebase was done incorrectly and the developer chose to take their old versions of the code base when they did their push, which resulted in a majority of my code over the last few days being overwritten.  
  
## The Epiphany
  
As I went through the task of restoring my changes from the previous day, the first thing that I did was restore the Unit Tests first. I restored the Unit Tests first, not because I thought they could help me with restoring the codebase but it felt like a natural start since TeamCity had showed me that lots of Unit Tests were missing. Once I finished restoring the Unit Tests, I ran the Unit Tests using [Test Driven.Net](http://www.testdriven.net/quickstart.aspx "Test Driven.Net"), got lots of failed tests and I immediately I had an Epiphany. The Epiphany was that the failed Unit Tests could be used to give me much needed insight into other functionality in the system that had been overwritten. Since I had extremely high code coverage, sometimes as high as 97% for some C# classes, I was able to use all the failed tests to help me track down functionality that was no longer present in the system.  
  
Git and a fair amount of looking at files manually played a role in the successful restore of code erroneously overwritten. However, I just thought that it was pretty cool that when I restored the Testing Suite first, the failed tests served as a another way to pinpoint what was overwritten in the system and more importantly, where those pieces of functionality needed to be restored.  
  
The above is one of the many reasons why I simply love Unit Tests!
