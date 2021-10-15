---
layout: post
title:  "Bored? Take the Tic Tac Toe Challenge!"
date:  2016-11-25
categories: [technology]
images: michael-d-green-grenitausconsulting-com-bored-take-the-tic-tac-toe-challenge-001.jpg
tags: [c#, fluentassertions, nunit, visual studio]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-bored-take-the-tic-tac-toe-challenge-001.jpg)

After eating Thanksgiving dinner with my family and returning to my house, I got a little bored.  
  
A friend of mine just started a IT Consulting Company a few months ago. I was browsing his public GitHub Page the day before Thanksgiving and I noticed that one of his repositories was a coding challenge for interviews. There were 2 coding challenges and one of them was to determine a winner of a Tic Tac Toe game.  
  
I had never attempted to do this, so I decided to spend a little time and take the Tic Tac Toe challenge. **Completed Solution on GitHub** [here](https://github.com/michaeldeongreen/TicTacToe)  
  
**Tech Stack**  
  
* C# 6
* FluentAssertions
* NUnit 3.5
* TestDriven.Net
* Visual Studio 2015

**Insight into how I approach problems**  
  
[![michael-d-green-grenitausconsulting-com-bored-take-the-tic-tac-toe-challenge-002](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-bored-take-the-tic-tac-toe-challenge-002.png)](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michael-d-green-grenitausconsulting-com-bored-take-the-tic-tac-toe-challenge-002.png)  
  
**Thoughts**  
  
* For the GameResultService, I probably should remove the constructor and pass the board into the GetResults Method. The coding challenge description seemed to indicated that you would just be given a board at a given point in time. I would need to make the change I have suggested, if it was interactive and I wanted to use the same service throughout the course of the game.
* I suspect that a lot of people would have come up with a nice algorithm that was more efficient and required less code.
* I thought about slapping a WinForms or even an AngularJS Front-End on it but I guess laziness is greater than boredom.

**Edit:**  
  
* Forgot to mention, this solution assumes a 3x3 Game Board, if I have the ambition, I may attempt to make it N x N **(2016-11-25)**
* Looks like I forgot to code for "Deadlock" **(2016-11-25)**
* Added Deadlock logic to the application **(2016-11-25)**
* Passing the board into GameResultService.GameStatus method, so you don't have to create a new service instance to continually check the board **(2016-11-25)**
* Created a new property on TicTacToeResult call Status that called overrided ToString that prints out the status of the game, so you don't have to have IF statements to check **(2016-11-25)**
* IsWinner boolean removed in place of a enum value Winner, NoWinner and Deadlock. Went back and forth on enum vs add a new boolean called IsDeadlock. Went with enum even though I believe enums should be mutually exclusive. Technically, the results can be NoWinner and Deadlock. **(2016-11-25)**
