---
author: Jason Tally
pubDatetime: 2024-07-08
modDatetime: 2024-07-08
title: Loading Linux on the VMware Edge 620
featured: false
draft: true
description: Loading Debian on the VMware Edge 620 (AKA Dell VEP 1425), and
  performing basic setup as a network device
---
# Why are we doing this

## Linux as the underpinning of networking

If you start digging into the inter workings of various network devices, you will start to find that even enterprise grade network devices, that would have traditionally run a bespoke operating system, are mostly running GNU/Linux underneath. Network device vendors will cover over the CLI and interfaces and in some cases implement their own data plane but much of the kernel and user space is still involved in critical functions of the device.

## Risks of old GNU/Linux on network devices

If you have the opportunity to use enterprise networking gear, one of the things you might notice is that network vendors are slow to update the linux kernel and other related software packages. This year, I loaded a beta version of a widely used enterprise network OS that will be released later in 2024 and was shocked that it was running the 3.10 linux kernel that was originally released more than a decade ago! Now some might argue that as long as the vendor is quick to patch security vulnerabilities, that this should be ok, because patches will be back ported to older versions of software. I would argue that we need to pay attention to at least two major methods of improving the security of software over time. The most visible of these two is patching of known vulnerabilities in software but the other is taking advantage of structural improvements of software that make it less likely to be at risk of unknown vulnerabilities. One of the main signals that we can see of this second case is how quickly a vendor updates underlying components of the solution with the version of the Linux kernel being a good starting point.

## Getting more familiar with GNU/Linux as a network OS

These two things, along with advancements like and eBPF and VPP with Linux Control Plane, make me want to get more familiar with running GNU/Linux as a network OS. I hope to gain more knowledge about the software that is underpinning all of the devices that I am architecting and also to glimpse into a future where we need to directly operate our network OS to stay secure.

# Hardware choice

Driven by cost while still wanting to use a widely deployed hardware platform that was designed specifically for networking, I chose to purchase a second hand VMware Edge 620 on ebay for $80 USD for this project. This is almost identical to the Dell VEP 1400 hardware and runs an Intel C3000 series system on chip that is used in other things like the Cisco 8200 series routers. The hardware provides 6 10/100/1000Base-T ports and 2 SFP+ ports all connected to decent Intel NICs. Despite being an Intel x86 box, it lacks any form of video out so our interactions will need to be entirely over serial console or SSH.