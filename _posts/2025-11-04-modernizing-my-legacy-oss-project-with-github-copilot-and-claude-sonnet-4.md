---
layout: post
title:  "Modernizing My Legacy OSS Project With GitHub Copilot And Claude Sonnet 4"
date:   2025-11-04
categories: [technology]
images: modernizing-my-legacy-oss-project-with-github-copilot-and-claude-sonnet-4-001.jpg
tags: [AI, Claude Sonnet 4, GitHub Copilot, Microsoft, Modernization]
--- 

![Modernizing My Legacy OSS Project With GitHub Copilot And Claude Sonnet 4!](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/modernizing-my-legacy-oss-project-with-github-copilot-and-claude-sonnet-4-001.jpg)

**Happy almost 2026!** It’s been a while since my last blog, and I’m excited to share something new. Recently, I’ve been exploring the world of *Vibe Coding* and decided to write about my experience. Although I’ve been a Software Developer for many years, I only recently started actively using GitHub Copilot to assist with my projects. There’s been plenty of buzz about Vibe Coding and predictions that developers might soon be replaced en masse. This past weekend, I put GitHub Copilot to the test by modernizing and refactoring one of my old OSS projects. In this post, I’ll share what the project needed, how I approached it, and my thoughts on Vibe Coding and GitHub Copilot.

# OSS Project Background

The application I targeted for my Vibe Coding deep dive is a roughly 5-year-old .NET 5 ASP.NET Web Application. I originally built this app while working in engineering for two large Microsoft Media & Comms customers. It leverages a mix of technologies, including:

- **ARM**
- **ASP.NET Core**
- **Azure**
- **Bash**
- **Blazor**
- **BlazorStrap**
- **Docker**
- **Terraform**

You can find the project here: [https://github.com/azure-samples/eventgrid-viewer-blazor](https://github.com/azure-samples/eventgrid-viewer-blazor).

The application allows users to view Azure EventGrid messages in real time, which is especially useful for observability and debugging scenarios.

Previously, the app supported Entra ID authentication controlled by a feature flag and integrated with Azure Key Vault for secure secret management. I’ve temporarily removed this feature during the recent changes but plan to reintroduce it in the near future.

I still receive questions about whether I’m maintaining this project, which made it an ideal candidate for a deep dive into Vibe Coding and modernization.

# What Needed to Be Done?

Initially, my goal was simple: update the BlazorStrap front end, upgrade the .NET version, and refresh the Azure EventGrid SDK. However, as I became more familiar with GitHub Copilot, it quickly became clear that I could go beyond these basic updates. 

I expanded the scope to include:
- Adding **robust logging** for better observability
- Converting the app to use **Azure Developer CLI** for streamlined workflows
- Modernizing the **documentation** for clarity and maintainability
- Removing **authentication features** (including Entra ID and Key Vault) to focus on building a clean **Vibe Code MVP**

# How Did I Go About It?

I started out using GPT-5, but in my experience, Claude Sonnet 4 felt more accurate and faster. The first step was having GitHub Copilot restructure the code so the application could run seamlessly in Visual Studio Code as well as Visual Studio, where it was originally built.

Next, I asked Copilot to upgrade the .NET version and ensure all existing Unit Tests passed after the changes. I was pleasantly surprised by how efficiently AI handled this task. While it introduced breaking changes, Copilot iteratively resolved them, leaving me with code that compiled successfully.

After that, I focused on updating the BlazorStrap front end—a task I had avoided in the past since front-end work isn’t my favorite. This was one reason I hadn’t maintained the project after leaving my previous engineering org. Copilot impressed me again: when stylesheet issues appeared, I described the problems conversationally, and it opened the app in a Simple Browser tab inside VS Code, let me select the problematic element, identified the CSS issues, and fixed them.

Once the app compiled and ran, I turned to deployment. Initially, I followed the old manual steps, but then realized: *Why not modernize the deployment process with AI assistance?* I merged the basic changes into the main branch and decided to convert the project to use Azure Developer CLI.

Before starting that conversion, Copilot amazed me again by diagnosing and fixing a race-condition bug that had existed since the project’s inception. For the CLI migration, Copilot used the existing ARM template behind the “Deploy to Azure” button to generate Bicep files. It introduced a bug by setting the .NET version to “.NetFramework,” causing silent deployment failures. After providing detailed troubleshooting context, Copilot identified and fixed the issue.

With the CLI conversion complete, I added basic logging, cleaned up and modernized documentation, and removed unused artifacts. I also asked Copilot to strip out all Entra ID authentication code so I could focus on the conversion in a dedicated feature branch.

Throughout the process:
- Copilot ran Unit Tests after each change.
- I reviewed most generated code and asked for explanations when implementations weren’t clear.
- When I requested logging, Copilot initially defaulted to Serilog and produced unnecessary code. I rolled back and had it implement built-in .NET Core Logging instead.
- Similarly, Copilot assumed MSTest for unit tests but adapted to xUnit after clarification.

Eventually, I merged the changes into the main branch once I was confident I could explain every modification.

### Summary of Changes
- Updated **.NET 5 → .NET 9**
- Updated **BlazorStrap 1 → 5**
- Updated **Azure EventGrid SDK 1 → 3**
- Fixed a pre-existing **race condition**
- Migrated to **Azure Developer CLI**
- Updated **ARM templates**
- Updated (and later removed) **Bash scripts**
- Updated **Unit Tests**
- Removed unused artifacts
- Implemented **logging**
- Modernized **documentation**

# What Are My Overall Thoughts?

After spending the weekend exploring **Vibe Coding**, I was completely blown away by what an AI assistant can do. I started as a skeptic, but this experience changed my perspective—I can’t imagine developing without a solution like **GitHub Copilot** going forward.

### Key Observations
- **Technical skill still matters.** I had to guide and correct Copilot, have an understanding of the .NET ecosystem, apply SOLID principles, and recognize what clean code looks like.
- **AI encouraged risk-taking.** Normally, I would have stuck to the original goal—updating libraries and ensuring the solution compiled. With Copilot, I confidently expanded the scope, even converting the project to use Azure Developer CLI.
- **Shift in IDE preference.** I’ve always considered Visual Studio superior for C# development due to debugging and IntelliSense. With Copilot integrated into VS Code, that advantage disappeared. I’ll likely use VS Code exclusively unless a client requires Visual Studio.
- **Control and transparency.** I felt in control throughout the process. Copilot answered questions about its decisions, and most explanations were clear and sufficient.
- **AI as a “large dog on a leash.”** Copilot is powerful but needs careful instruction from someone who understands the task. Without guidance, it can produce overly complex “slop” code that’s hard to justify in a code review.

### Final Thoughts
This experience reinforced that **AI coding assistants are the future**. Copilot accelerated the ideation-to-implementation loop, letting me focus on architecture and business logic rather than boilerplate. It has undoubtedly made me a better developer.

For those still skeptical, consider what Jensen Huang of NVIDIA said:  
> *“AI will not take your job, but a person who uses AI will.”*

I encourage my fellow technical colleagues to heed those words and act accordingly.