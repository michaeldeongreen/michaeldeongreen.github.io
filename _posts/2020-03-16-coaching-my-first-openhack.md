---
layout: post
title:  "Coaching My First OpenHack"
date:   2020-03-16
categories: [careeradvice]
tags: [career]
image: Production_Fundamentals_Coach.png
---

![Production Fundamentals Coach](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/Production_Fundamentals_Coach.png)

Over the past 4 months, I have had the opportunity to help create the latest [CSE OpenHack](https://microsoft.github.io/code-with-engineering-playbook/CSE.html) content and also serve as a coach for the first time, as our team held dry runs to preview our content to selected audiences inside of Microsoft.

## Background

For those who are unfamiliar with Microsoft OpenHacks, you can go [here](https://openhack.microsoft.com/) a to get a fairly good overview of what a Microsoft OpenHack is, who they target and what are some of the goals of an OpenHack.

The OpenHack that our team created focused on teaching attendees about CSE's [Core Engineering Principles](https://github.com/Microsoft/code-with-engineering-playbook), which involves concepts such as code quality, testing, process automation, how to prepare to go to production, observability and troubleshooting live-site issues.

## Reaching Consensus

Whenever you are part of a team that is creating content that will teach people core fundamentals of any industry, you would actually be very surprised at how many people have different ideas about what are “core” fundamentals. A subject such as Software Engineering Fundamentals can be very divisive because people have their own ideas about what this entails. Fortunately, even though people on our team had different and often times, “passionate” ideas about what are engineering fundamentals, everyone was able to voice their opinion openly, have respectful debates and ultimately, come to a working consensus as a team.

One of the great takeaways from this project is that when you are creating content that will help guide people on a particular subject, you have to really examine assumptions, reduce your own personal biases and understand the content you are producing from several angles.

## Not Everyone Works & Learns the Same Way

Another major takeaway from coaching my first OpenHack is that everyone has their own dev setup and learn in different ways. I know that this may sound like common sense but it was something that caught me a little off guard when I coached in the first dry run. For my development environment, I use Hyper-V to create Ubuntu (Preferred) or Windows Virtual Machines, depending upon the project. I do this so I don’t pollute my work laptop with tons of software and after a project is over, I can just delete the Virtual Machine.

During the OpenHack dry runs, you had people with Macs, PCs or PCs using [WSL](https://docs.microsoft.com/en-us/windows/wsl/about). People used Visual Studio Code, Visual Studio or a combination of the two. Another observation was that people tend to do something that is kind of a pet peeve of mine, which is having tons and tons of tabs open at once :). As a Coach you have to be somewhat knowledgeable about the different types of OSs and tools that people tend to use. Also, I had to really learn to not be judgemental. If someone preferred to use Visual Studio over Visual Studio Code, that is okay. If someone had a ton of tabs open, while helping them troubleshoot, don’t force them to close all of their tabs.

Because people tend to use a myriad of tools and OSs, I started looking into [Visual Studio Code Devcontainers](https://code.visualstudio.com/docs/remote/containers) as a way to get our attendees up and running quickly so they could spend as much time as possible focusing on the core concepts of the OpenHack and not local dev environment issues.

## Learn to Lean on Others

Another great take away from coaching my first OpenHack is that I had to learn to lean on others and although it sounds arrogant, I “don’t” know everything (the horror!). As I was telling one of the attendees after the first dry run, I was extremely nervous going into coaching my first OpenHack. I have never been a position where I was “formally” seen as a guide and even if I know 99% of the material, I will still zero in on the 1% I feel I don’t know and build up unnecessary panic (can you say [Impostor Syndrome](https://en.wikipedia.org/wiki/Impostor_syndrome)!).

Furthermore, throughout my career, I have always had this weird idea that when you have to ask someone a question that I believe I should know and/or cannot find out on my own, then that is kind of a sign of weakness and the other person will think ill of me.

This assumption is quite ignorant, arrogant and cannot be further from the truth. During the creation of the Production Fundamentals OpenHack, each coach started to develop specific areas of expertise. During the dry runs, I definitely had to lean on the other coaches when the attendees arrived at challenges that I was less familiar with and vice versa.

## Attendees May Know More than You

Similar to the idea of “I don’t know it all”, in some cases the attendees could be more knowledgeable on a subject than the coach in certain areas. As stated prior, I primarily use Ubuntu for my development environment. While coaching the OpenHacks, I had to lean on the attendees as we troubleshot issues with Macs or [WSL](https://docs.microsoft.com/en-us/windows/wsl/about). During the Production Fundamentals OpenHack, I became extremely comfortable with the fact that some of the attendees knew more than I did on a subject and in some cases, were experts on some of the content.

But that is okay because during an OpenHack learning doesn’t just flow one way, ie attendees learning from the coaches. On any project, everyone can learn from everyone, even if it is a Tech Lead learning from an Individual Contributor or in the case of an OpenHack, a coach learning from the experiences of the attendees.
