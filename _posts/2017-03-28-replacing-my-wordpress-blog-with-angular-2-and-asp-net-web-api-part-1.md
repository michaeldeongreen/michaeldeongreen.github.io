---
layout: post
title:  "Replacing My Wordpress Blog with Angular 2 and ASP.Net Web API Part 1"
date:  2017-03-28
categories: [technology]
images: replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api.png
tags: [ASP.Net Web API, Angular 2, NUnit, CSS, TypeScript, Visual Studio 2015, C#, StructureMap, Json.net, Bootstrap 3, SEO, angular-cli]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api.png)

I recently decided to build upon my Angular 2 skills by ditching my Wordpress blog and writing a completely custom blog solution in Angular 2. I wanted to share my experiences in a 3 part blog series. In part 1, I will be talking about why I decided to ditch Wordpress and an overview of the Angular 2 solution.  
  
**Why did I ditch Wordpress?**  
  
The main reason why I ditched Wordpress was because it was just too slow. I started my first Wordpress site about 2 years and it was hosted with Arvixe. My Wordpress blog on Arvixe was slow and constantly timed out. A few months ago, I had enough of the timeout issues and decided to move my blog to Godaddy. My Wordpress with Godaddy never timed out but was extremely slow. My business [homepage](http://grenitausconsulting.com/) was also hosted with Godaddy and because of its simplicity, it loaded pretty fast.  
  
Eventually, I called Godaddy to ask if there was something they could do about the slow load times with my blog and of course, they told me that there was nothing that they could do because Wordpress was just slow. The technician I talked with told me to look at some tutorials on how to make Wordpress faster. Although I was very disappointed, I took the technician's advice and started researching ways to make Wordpress faster.  
  
When I started to research how to make Wordpress faster, I was pretty surprised that there so many people asking the same question I was asking. I eventually found my way to Tom Dupuis' wonderful [blog](http://www.onlinemediamasters.com/slow-wordpress-hosting-godaddy/#wp-optimize) about using caching and Cloudflare to optomize Wordpress. So I ended up spending half a Saturday implementing W3 Total Cache and using Cloudflare to boost make Wordpress faster.  
  
The first thing Tom Dupuis tells you to do is to use [gtmetrix.com](http://gtmetrix.com) to get initial performance numbers for your Wordpress site before you make any changes. Unfortunately, I didn't save a link to my performance numbers before using W3 Cache Toal and Cloudflare but you can find the link to the snapshot of the performance numbers after I started using cache and Cloudeflare[here](https://gtmetrix.com/reports/Blog.Michaeldeongreen.com/2yNIESxy).  
  
As you can see, the numbers are not very impressive and each blog post was slow until you visited the post the first time, so it could be cached and this was only temporary. Also, per Tom Dupuis' blog, I didn't cache any of the Wordpress Admin Pages for obvious reasons, so it was excruciatingly slow.  
  
I started trying to learn Angular 2 last year and as I talked about in [this](https://blog.michaeldeongreen.com/technology/2017/03/21/upgrading-my-homepage-to-angular-2-using-angular-cli.html) blog, I had recently re-written my [homepage](http://grenitausconsulting.com/) to use Angular 2. Since I had already used Angular 2 for my website, it was only natural that it would be the Front-end framework of choice to help me solve my poor performing Wordpress problem.  
  
**A Checklist of Must Haves!**  
  
Before I wrote any code and committed to the project, I went about making a checklist of features that Wordpress provided that I would need in the Angular 2 solution.  
  
* **Preview** - In Wordpress, there was a plug-in called [WP-DraftsForFriends](https://wordpress.org/plugins/wp-draftsforfriends/) that I used to share blog drafts with co-worker before it was officially released. This functionality was matter of writing the code, so I wasn't too worried about implementing this feature.
* **Prismjs** - I used the [Prismjs](http://prismjs.com/) plug-in in Wordpress for code syntax highlighting and I was very happy that I could use this in a stand alone HTML based application
* **Sitemap** - Like many bloggers, I used the [Yoast SEO](https://yoast.com/) plug-in for SEO. I ended up creating a Windows Console Application to handle the creation of Sitemaps and I will talk about this tool later.
* **RSS** - Out of the box, Wordpress gives you RSS Feeds and though I didn't use this feature, I found that I wanted it for my Angular 2 solution. I ended up writing a custom solution to generate RSS XML files, which also was placed in the same Windows Console Application that I used for Sitemap generation.
* **Social Media Share** - In Wordpress, I used the plug-in called [Simple Share Buttons Adder](https://wordpress.org/plugins/simple-share-buttons-adder/) to allow my users to easily share my blogs on Social Media. I was pleased to find that there were a lot sites where you could find free Social Media icons and that all you needed to get share buttons to work was to place a predefined querystring at the end of each Social Media url.
* **Speed** - One of the main driver for me taking on this project was so my blog didn't take 6+ seconds to load. The solution that I decided to implement would have no database, it would be a single page application and all of the data would be in-memory, so I was confident that it would be much faster.

**50 ft View**  
  
When the solution was completed, I ended up with 3 major components, increased knowledge of Angular 2, Front-end development and SEO. The project took about 2.5 weeks to complete and I am very please with the end results. [Here](https://gtmetrix.com/reports/Blog.Michaeldeongreen.com/1S5EROfu) is a link to the gtmetrix snapshot after the Angular 2 and Web API solution was implemented. As you can see, the performance metrics are much improved after implementing my custom solution. Also, below is a diagram of the components I created to complete the project:  
  
![replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-1-002.png](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/replacing-my-wordpress-blog-with-angular-2-and-asp-net-web-api-part-1-002.png)  
  
**The Components**  
  
**The Client** - The client is the Front-end solution that uses Angular 2, Bootstrap 3, CSS and HTML5.  
  
**The Web API** - The Web API exposes several Endpoints to serve up in-memory data such as Posts, HTML strings, Tags and Categories. I used Visual Studio 2015, C#, NUnit, StructureMap and Json.net to implement the Back-end solution.  
  
**gc-cli** - gc-cli (gc stands for "Grenitaus Consulting", I know, very _cheesy_) is a Windows Console Application that I created, which contains C# libraries that will generate Sitemaps, an RSS XML file and static HTML pages for SEO. The path to the executable was set in my Windows $PATH so I could use Bash to type in commands such as "gc-cli build -prod" to produce Production artifacts.  
  
Over the next couple of blogs, I will be going into greater detail about these 3 components.  
  
**Conclusion!**  
  
Even though it was incredibly frustrating at times and I probably got some new grey hairs, it was a very fun experience writing custom software to serve my blogging needs. I learned a lot about Angular 2, SEO, Front-end development, TypeScript and Single Page Applications in the process. I look forward to sharing some of my trials and tribulations in this 3 part blog series.
