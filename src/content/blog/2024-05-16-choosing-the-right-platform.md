---
author: Jason Tally
pubDatetime: 2024-05-16
modDatetime: 2024-05-16
title: Choosing the Right Platform
featured: false
draft: false
tags:
  - principles
  - content
description: Key Principles I Used to Select the Platform for My New Home for Content
---
Back in the day, I used a WordPress-based platform to share my professional thoughts. As kids and work consumed more of my life, I decommissioned the blog due to the time and expense required to maintain it. Fast forward more than a decade, and I have moved from being an individual contributor into roles that involve forming consensus, teaching, and leading others. These newer roles have made me value sharing my ideas more. For that reason, I started looking for a more modern platform to host my home for professional content. Here are some of the principles I considered when selecting the components for this site.

# Security

<p style="text-align: start">Over the years, WordPress and its plugins, along with other dynamic content platforms, have had many vulnerabilities. I want a platform that won't require me to jump on patches or remediation instantly to keep it online or to protect visitors. Much of the attack surface of a dynamic content platform comes from the fact that it is running a scripting language. Static content platforms have since emerged (and started to die out a bit) that have some of the benefits of a dynamic platform (not needing to create each page by hand and cross-link it from others) while skipping the dynamic component, significantly reducing the attack surface.</p>

# Performance

<p style="text-align: start">Dynamic content platforms are slow to generate their pages on the server, resulting in the need for server-side cache functions or CDN-based caches. More modern platforms can rely heavily on JavaScript on the client side to perform similar work, also causing slowness for readers. From the host to the site generator, I want to use something that will serve the content quickly and provide content that renders quickly in the browser.</p>

# Simplicity and User Experience

<p style="text-align: start">I want to be able to reuse some or all of the components of the platform for other purposes, where I may be supporting others who aren't familiar with Markdown, npm, or Git. I also want to be able to support authors that only have access to mobile devices. This is a problem for static site generators in general. One way to solve this has been to have a static site that pulls content from a dynamic content management system. I chose to avoid that pattern due to the complexity of having a static site interact with a CMS.</p>

# Efficiency

<p style="text-align: start">I will end up using this platform for more than just a single site, so efficiency of recurring operating costs will add up over time. I would like the content I am posting to be able to stay online for decades with minimal recurring cost while still being performant, secure, and scalable.</p><p style="text-align: start"><a href="https://jasontally.com/posts/2024-05-18-cloudflare-pages-github-pages-cms-and-astropaper/">In the next post</a> I explain the different components that I chose based on these principles.</p>