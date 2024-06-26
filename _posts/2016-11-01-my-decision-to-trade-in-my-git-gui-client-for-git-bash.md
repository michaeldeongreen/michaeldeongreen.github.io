---
layout: post
title:  "My Decision to Trade in my Git GUI Client for Git Bash!"
date:  2016-11-01
categories: [technology]
images: my-decision-to-trade-in-my-git-gui-client-for-git-bash-git-windows-logo.png
tags: [tortoisegit, git, git bash]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/my-decision-to-trade-in-my-git-gui-client-for-git-bash-git-windows-logo.png)

In this blog, I wanted to discuss my decision to trade in my Git GUI Client for Git Bash. I would like to discuss some of the reasons why I have been using a Git GUI Client and why I have ultimately decided to start using Git Bash.  
  
**Why did I start using a Git GUI Client?**  
  
Before I joined my current consulting company, I had never used Git. All of the places that I had worked were either Microsoft shops that used TFS or they used Subversion. When managing my code with TFS, I would obviously use Visual Studio, a very well known IDE. When I worked for companies that used Subversion, I would use TortoiseSVN because this is what everybody on the team used :). Anybody that knows anything about centralized version control systems, understands that these systems aren't all that complicated when strictly focusing on code management. Because of this, there really isn't a strong need to understand what is really going on underneath the hood, you grab your changes from the central repository, make changes locally and you check them into the central repository.  
  
When I joined my current company almost 2.5 years ago, I was first introduced to Git. I was notified that a co-worker had decided to leave the company and not only did I have to head to California the next week, I had to learn Git, so I could quickly get up to speed on the code base for knowledge transfer. When I was first introduced to Git, like many, I found it very confusing, oddly different from the centralized version control systems that I was used too and I was definitely not confident enough to manage my code using a Git CLI. So naturally, I went on over to Tortoise's website and was very pleased that they had a Git GUI client and it was very similar to TortoiseSVN. So over the next 2.5 years, I used TortoiseGit exclusively to manage my code and I even wrote a blog about how to use it to convert a SVN repository to Stash, which you can find [here](https://blog.michaeldeongreen.com/technology/2018/02/08/how-to-convert-an-svn-repository-to-stash-using-tortoisegit.html).  
  
**Why did I start using Git Bash?**  
  
One thing I wanted to be very careful of, was to ensure that I was making this transition for the right reasons. As I explained to one of my friends, who is an Software Architect himself, I didn't want to start using Git Bash solely because I wanted to be one of the "cool" kids (now I can't get Echosmith's song out of my head).  
  
Although TortoiseGit is a great product, I felt that because it abstracted all of the Git commands, that I really didn't understand how Git worked and if you don't understand how something works, you really don't know it very well. I had a conversation with a co-worker and we talked about how those who use Git from the command line have more knowledge about how Git works and he believed you should start using the command line first and then a Git GUI client. As someone who loves CLI tools and has written a few open source software based upon CLI tools, I wholeheartedly agreed but I waited an entire year to eat my own dog food.  
  
Last week I decided to put my money where my mouth is and slowly start the journey of replacing my Git GUI client for Git Bash. The main reason why I chose Git Bash over PowerShell was because I was already using Git Bash for NodeJS and I felt that it was closer to how I use my dedicated Ubuntu Linux Server. I have a few open source projects that I work on and I thought those would be great candidates to start using Git Bash.  
  
What is so great about using Git commands is that you will naturally have to review documentation on what the commands do and more importantly, what parameters are needed and how those work as well. I have been using a Git GUI client for over 2.5 years, so I had a very decent handle on basic Git concepts. However, when I started to use Git Bash, I found that I really didn't have very deep knowledge about Git and it has been an absolute joy to actually understand what my Git GUI client has been doing behind the scenes for me. I also found that I didn't have a strong handle on Git configuration files, where they were stored and just how flexible Git really is.  
  
One issue that seemed to always come up as a reason not to use the command line was merging and resolving merging conflicts. A few days ago, I was talking to one of my co-workers about the built in merge tool and he was like, "You know you can change the default merge tool right?". After doing a quick Google search, I was able to configure Git (from the command line) to use TortoiseGit's tortoisemerge as the default merge tool to resolve conflicts. Later that day, I was talking to a former co-worker while driving home and he told me that I could actually switch out the Git Core Editor and as soon as I got home, (again, using the command line) I configured Git to use Notepad++ as my global core editor.  
  
I used my personal laptop to transition into using Git Bash, so I needed to apply the same Git global settings to my client's laptop and my home PC, where I do most of my development. As a person who hates redundancy, I decided to write my first Bash script to automate this process. What this script does is:  
  
* Checks to see if TortoiseGit is installed. If so, it sets tortoisemerge as the global merge tool
* It then configures Git to not keep backup (.orig) files
* Next, it will check to see if Notepad++ is installed. If so, it sets Notepad++ as the global core editor

If the Bash scripts finds installed software, it will output the results in green, if it does not, the output will be red.  
  
Below is the simple Bash script that I wrote:  
  
```bash
    tortoisegit="C:/Program Files/TortoiseGit"
    
    if [ -d "$tortoisegit" ];
    then
     echo -e "---\e[32m---TortoiseGit is installed.---\e[0m"
     echo "---Setting Git Merge Tool to tortoisemerge---"
     git config --global merge.tool tortoisemerge 
     echo "---Configuring Git to not keep (.orig) back up files---"
     git config --global mergetool.keepBackup false
    else
     echo "\e[1;31mTortoiseGit was not found at location: $tortoisegit\e[0m"
    fi
    
    notepadpp="C:/Program Files (x86)/Notepad++/notepad++.exe"
    
    if [ -f "$notepadpp" ];
    then
     echo -e "\e[32m---Notepad++ is installed.---\e[0m"
     echo "Setting the Default Core Editor to Notepad++---"
     git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin" 
    else
     echo -e "\e[1;31mNotepad++ is not installed.\e[0m"
    fi
```

**Final Thoughts**  
  
As of roughly 4 days ago, I have been using Git Bash 100% for all of my personal and work projects. I have found that switching to the Git command line has made me more knowledgeable about how Git works, how to configure Git and a better understanding of what commands to use in certain situations. Entire books have been written about Git and I am far from an expert but I am very happy that I decided to start using the command line and will continue to do so going forward.
