---
author: Jason Tally
pubDatetime: 2024-05-18
modDatetime: 2024-05-18
title: Cloudflare Pages, Github, Pages CMS and AstroPaper
featured: false
draft: true
description: "Choosing the Right Platform Follow-up: Cloudflare Pages, Github,
  Pages CMS and AstroPaper"
---
In [Choosing the Right Platform](https://jasontally.com/posts/2024-05-16-choosing-the-right-platform/), I covered principles I used to select the platform for my new home for content. Now we will take a look at what I chose and some of the reasons behind those selections.

# Host

Knowing that I primarily want to use a static site for performance and security reasons, I considered some of the top static site hosting providers like S3, Github Pages and Cloudflare Pages. I avoided looking at Netlify and Vercel because they are primarily a layer on top of other providers, violating a couple of my overall architectural principals, and because the sponsorship money, and the undue influence it has, freaks me out a bit.

My aim to provide user/mobile friendly posting of content also had me looking for something in the chain that could run the static site build. Github Actions and Cloudflare Pages both seem to be good options here.

I ultimately chose Cloudflare Pages because of its tight integration to other Cloudflare services, great performance, globally distributed nature and options for mixing static and dynamic content with functions. If I had to move off Cloudflare Pages I would probably reach fist for something like Github Actions for the build stage and potentially some form of S3 for the hosting.

# Repository

For the pre-build storage I briefly considered GitLab due to its more independent stance but security issues with GitLab since its inception have highlighted the difficulty of managing threats to that type of environment. Contrary to predictions, GitHub has continued to handle security well since its acquisition by Microsoft. I do with that GitHub would allow me to use only multiple FIDO2 keys for the second factor.

# Content Management

Static site generators have resulted in some over complicated headless CMS systems but also a new breed of content authoring options such as Tina CMS or the Prose markdown editor that help to manage markdown or other assets that feed into a static site generator. I chose to go with Pages CMS hosted in my own Cloudflare account. Pages CMS is primarily a Vue based app that runs in the browser along with a small server side component running on Cloudflare Pages Functions that helps with authentication to GitHub. Pages CMS helps with authoring markdown files and also helps with asset management for smaller assets that you would store directly in your GitHub repo.

# Site Generator

The final piece to connect these dots together is the site generator. For performance reasons I am currently using AstroPaper that is based on the Astro meta framework. I also tried an Eleventy theme but went with AstroPaper because it had better performance and needed less tweaking for my day 1 use cases. Of all the things, I think this is the most likely to change in the future. I’d love to get more experience using something like Remix (or are we calling it react router now?) that I could use to build a full stack edge app in the future, so I will keep my eyes out for remix starters that could help get me more practice there.