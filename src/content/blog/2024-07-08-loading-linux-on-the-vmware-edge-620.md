---
author: Jason Tally
pubDatetime: 2024-07-08
modDatetime: 2024-07-08
title: Loading Linux on the VMware Edge 620
featured: false
draft: true
description: Loading Debian on the VMware Edge 620 (AKA Dell VEP 1425), updating
  to the latest LTS kernel and performing basic setup as a network device
---
# Linux as the underpinning of networking

If you start digging into the inter workings of various network devices, you will start to find that even enterprise grade network devices, that would have traditionally run a bespoke operating system, are mostly running GNU/Linux underneath. Network device vendors will cover over the CLI and interfaces and in some cases implement their own data plane but much of the kernel and user space is still involved in critical functions of the device.

# Risks of old GNU/Linux on network devices

If you have the opportunity to use enterprise networking gear, one of the things you might notice is that network vendors are slow to update the linux kernel and other related software packages, missing out of structural security improvements that can’t be back ported. Well resourced attackers have taken notice