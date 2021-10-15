---
layout: post
title:  "My Azure DevOps Retrospectives Extension Review"
date:  2019-02-25
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Azure DevOps]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

Recently, I was asked by the the Tech Lead of a project that our team just recently wrapped up here at Microsoft to MC the final Sprint Retrospective. The Sprint Retrospective would include both the Microsoft Staff and Client Partner Staff, whom we assisted in implemetning various backend components in the Azure Cloud.  
  
As an engineer who is both a [PMP](https://www.pmi.org/certifications/types/project-management-pmp) and a [SCRUM Master](https://www.scrumalliance.org/get-certified/scrum-master-track/certified-scrummaster), I have taken part in many Sprint Retrospectives but I have never been asked to facilitate one. I wanted to find a tool that was easy to use, interactive and if possible, could be used in [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/). After doing some research, I found a Azure DevOps Extension that met all of my criteria. In this blog I want to write a review of the Azure DevOps [Retrospectives](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.team-retrospectives) extension.  
  
**Prerequisites!**  
  
* Access to a Azure Subscription
* Permissions to install Azure DevOps Extensions

**Overview of the Extension!**  
  
The Retrospectives extension is a Azure DevOps extension that can be used to host critical feedback during Sprint Retrospective meetings. The extension will allow the team to answer 3 fundamental Sprint Retrospective questions which are:  
  
* What went well?
* What didn't go well?
* How can we improve as a team?

As you will see, it provides swim lanes to answer the first 2 questions and allows the team to answer the thrid question, which is "How can we improve as a team?" by allowing the team to create Azure DevOps Backlog items directly based upon "What didn't go well?".  
  
**Installation!**  
  
First, you will want to install the Retrospectives extension. Use the animated gif below as a guide on how to install the extension **_(click gif to start from the beginning)_**:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-001.gif)  
  
**Exploring Retrospectives!**  
  
Once you have installed the Retrospectives extension, you will be presented with the screen below:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-003.png)  
  
Next, you will want to press the "Create Board" button to create a new retrospective. You will be presented with the popup window below, where you will give your retrospective a title, specify the order of the _swim lanes_ and to add up to 4 columns in total.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-004.png)  
  
After you save, you will see a screen similar to this:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-005.png)  
  
Now that we have a new retrospective, the team can use the _Collect_ tab to start adding items. See the animated gif below **_(click gif to start from the beginning)_**:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-006.gif)  
  
Next, the team can use the _Group_ tab to group similar feedback items to keep the retrospective swim lanes nice and tidy. Also note, that you can delete items and ungroup the items as well **_(click gif to start from the beginning)_**:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-007.gif)  
  
Next, the team can use the _Vote_ tab to vote on what are the most important feedback items **_(click gif to start from the beginning)_**.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-008.gif)  
  
After all of the feedback has been provided and votes cast, the team can use the _Act_ tab to review all of the feeback, sorted by the _number of votes_ to create _work items_ to literally take action. Also note that if the team wants to focus on one feedback item at a time, they can use the _Focus Mode_ to zero in on one feedback item at a time **_(click gif to start from the beginning)_**.  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-009.gif)  
  
**What I Love about the Retrospectives Extension!**  
  
**Intuitive** - I like how easy it is to get the extension installed _(Kudos to the Azure DevOps Team)_, the swim lanes and the extension is extremely intuitive. Also, you probably couldn't tell from the animated gifs but the feedback repsonses and groupings are real time.  
  
**Grouping** - I like the _group_ feature. During many Retrospectives, team members are likely to provide similar feedback and it is nice to be able to group the items to keep the swim lanes from being cluttered with duplicate feedback items.  
  
**Voting** - I not only like the ability to vote on feedback items but the extension automatically sorts feedback items by the number of votes when you navigate to the _Act_ tab. This feature is expecially useful if you may not have enough time to go over all feedback items and in such cases, you can ensure that the feedback items with the most votes are discussed first.  
  
**Act** - At first, I didn't quite understand the _Act_ tab but once I started to use the Retrospectives extension, I felt that the _Act_ tab was brilliant. I thought that the _Act_ tab should just be a swim lane like the other 2 fundamental questions but after becoming more familiar with the Retrospectives extension, I felt that it was extremely useful to be able to tie _What didn't go well_ feedback items back to actual actionable Backlog items, to force the team to _Act_ on the feedback given during the Retrospective.  
  
**Positive Feedback!**  
  
**Can't take back vote** - If a team member accidentally votes on the wrong feedback item, there is no way to withdraw the vote.  
  
**Unlimited votes** - As of now, a user can cast ulimited votes for a feedback item and can falsely influence the weight of a feedback item.  
  
**Can't expand grouped items in Focus Mode** - If your team decides to use the _Focus Mode_ tab to zero in on each feedback item, you cannot expand grouped items. This feature is useful because you may want to see which team members added the feedback to ask them to talk about it. At the moment, only the top level feedback can be viewed and when I ran my first Retrospective using this extension, I just used the _Act_ tab view when the team started to discuss each feedback item.  
  
**No Anonymous Option** - At this time, there isn't a way to provide feedback anonymously. Typically, anonymity isn't a big deal for teams that have worked together for several Sprints and most of the team gets along. However, if the team is new, there is hierarchy on the team (ie the boss is a team member) or the team members don't like each other, anonymity is a pretty big deal and can be used to spur much needed conversations to improve the overall morale of the team.  
  
**Uninstalling the Retrospectives Extension!**  
  
Last but not least, the animiated gif below demonstrates how to uninstall the Retrospectives extension **_(click gif to start from the beginning)_**:  
  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-azure-devops-retrospectives-extension-review-002.gif)
