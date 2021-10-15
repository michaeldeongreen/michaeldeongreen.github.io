---
layout: post
title:  "It's All in the Name!"
date:  2018-08-05
categories: [technology]
images: michaeldeongreen-logo.png
tags: [SCRUM, Unit Testing]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

I wanted to take some time to write a very short blog entry about an experience I recently had that further strengthened my belief that code readability is extremely important.  
  
Recently, one of my clients has started to really focus on the importance on writing Unit Tests. This client has gone as far as to mandate that even members of QA write a certain amount of Unit Test per quarter.  
  
For the last few Sprints, our team has had to focus on implementing features to become [EI3PA](https://www.swordshield.com/experian-ei3pa-compliance) compliant. In particular, I recently heavily re-factored their existing audit logging framework as it was extremely inflexible and not extensible.  
  
[Martin Fowler](https://en.wikiquote.org/wiki/Martin_Fowler) once stated, "Any fool can write code that a computer can understand. Good programmers write code that humans can understand.". I am a huge believer in this basic concept and in the last few years, I have gotten a lot better at writing code that is easier for a human to follow.  
  
As I re-factored my client's existing audit logging framework, I took care in how I named "things" such as constants, variables, methods, abstracts and sub classes. In fact, I could make the argument that I paid as much attention to how I named "things" as I did making the heavily re-factored audit logging framework adhere to [SOLID](https://en.wikipedia.org/wiki/SOLID) Principles.  
  
During one of our [SCRUM](https://en.wikipedia.org/wiki/Scrum_(software_development)) stand ups, our QA Team Member mentioned that she is required to write Unit Tests when the opportunity presents itself. She asked me if I could help her write Unit Tests for the re-factored audit logging framework, since she was going to be testing my changes.  
  
Since I work remote, I shared my screen with her and guided her on how to write new Unit Tests (me and another team member had already written a lot Unit Tests) for my changes. She begin to ask me about the changes that I made and why I needed to make these changes. So since I already had my screen shared, I actually went into the code and started showing her my changes.  
  
As I continued to explain to her how the code worked, I noticed something interesting, within a few minutes, I really didn't need to continue to explain anything to her because since I took great care in how I named "things" in my code, a human being, who had never laid eyes on my code before could easily follow the code.  
  
As a member of QA, she has a ton of domain knowledge and a very strong understanding of how the components that our team is responsible for works. Because of this fact, it was very interesting to hear her say things like, "Oh okay, I can understand this code or now I know where the code is that performs this business logic or this is the code that determines what gets audited when I press a button on a screen!".  
  
I am a big proponent of writing human readable code. When you take care in how you write your code, it makes it easier for future you and the person that comes after you to make changes because the code will be more easier to understand. Also, in the future, you never know who is going to open up the hood and see what you have done!
