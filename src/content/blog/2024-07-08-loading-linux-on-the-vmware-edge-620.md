---
author: Jason Tally
pubDatetime: 2024-07-08
modDatetime: 2024-07-08
title: Loading Linux on the VMware Edge 620
featured: false
draft: true
description: Loading Linux (Debian) on the VMware Edge 620 (AKA Dell VEP 1425),
  updating to the latest LTS kernel and performing basic setup as a network
  device
---
# Linux as the underpinning of networking

If you start digging into the inter workings of various network devices, you will start to find that even enterprise grade network devices are mostly running linux underneath. Network device vendors will cover over the linux CLI and interfaces and in some cases implement their own data plane but much of the kernel and user space is still involved in many critical functions of the device.