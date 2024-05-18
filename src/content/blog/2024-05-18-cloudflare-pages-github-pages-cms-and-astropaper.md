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

I ultimately chose Cloudflare Pages because of its tight integration to other Cloudflare services, great performance and also options for mixing static and dynamic content with functions. Cloudflare Pages also provides the build environment, removing

Repository

CMS

Site Generator