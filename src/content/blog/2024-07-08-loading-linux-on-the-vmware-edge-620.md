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

If you have the opportunity to use enterprise networking gear, one of the things you might notice is that network vendors are slow to update the linux kernel and other related software packages. This year, I loaded a beta version of a widely used enterprise network OS that will be released later in 2024 and was shocked that it was running the 3.10 linux kernel that was originally released more than a decade ago! Now, some might argue that as long as the vendor is quick to patch security vulnerabilities, that this should be ok, because patches can be back ported to older versions of software. I would argue that we need to pay attention to at least two major methods of improving the security of software over time. The most visible of these two is patching of known vulnerabilities in software but the other is taking advantage of structural improvements of software that make it less likely to be at risk of unknown vulnerabilities. One of the main signals that we can see of this second case is how quickly a vendor updates underlying components of the solution. The version of the Linux kernel, version of OpenSSL and versions of web server software can be quick indicators of how well a vendor is modernizing their platform.

## Getting more familiar with GNU/Linux as a network OS

These two things, along with advancements like and eBPF and VPP with Linux Control Plane, make me want to get more familiar with running GNU/Linux as a network OS. I hope to gain more knowledge about the software that is underpinning all of the devices that I am architecting and also to glimpse into a future where we need to directly operate our network OS to stay secure.

# Hardware choice

Driven by cost while still wanting to use a widely deployed hardware platform that was designed specifically for networking, I chose to purchase a second hand VMware Edge 620 on ebay for $80 USD for this project. This is almost identical to the Dell VEP 1400 hardware and runs an Intel C3000 series system on chip that is used in other things like the Cisco 8200 series routers. The hardware provides 4 Intel I350 NIC’s connected to Base-T ports 1-4 and 4 Intel X553 NIC’s connected to Base-T ports 5, 6 and both SFP+ ports. It also provides a 16GB eMMC drive and 128GB M.2 SSD for storage. Despite being an Intel x86 box, it lacks any form of video out so our interactions will need to be entirely over serial console or SSH.

### Loading Dell DiagOS onto a USB drive

In order to use the VMware Edge 620 like a regular VEP 1400, we need to load the VEP 1400 BIOS onto the unit. For at least one reason (that it disables the hardware watchdog) one of the best ways to do this is to load the Dell DiagOS onto the eMMC, boot from that, and then use the Dell Unified Firmware Updater to convert our VMware Edge 620 into a VEP 1400.

After downloading the DiagOS image from Dell and extracting the ISO file, we will use dd on macOS or linux (if you use Windows try Rufus for this) to copy the ISO to a USB drive that we will later boot the VMware Edge 620 with.

Insert the USB drive into your computer, identify it ("diskutil list" on macOS) and then unmount it:

```
diskutil unmountDisk disk4
Unmount of all volumes on disk4 was successful
```

Copy the iso file to the USB drive:

```
sudo dd if=/Users/jason.tally/Downloads/diagos-recovery-x86_64-dellemc_vep1400_c3538-r0.3.43.3.81-27.iso of=/dev/disk4 bs=8m
34+0 records in
34+0 records out
285212672 bytes transferred in 41.290682 secs (6907434 bytes/sec)
```

Unmount the USB drive again:

```
diskutil unmountDisk disk4
Unmount of all volumes on disk4 was successful
```

Remove the drive it from your computer and insert it into one of the USB A ports on the side of the VMware Edge 620

## Connecting to the serial console

The VMware Edge 620 series and VEP 1400 series come with a built in USB to serial convertor that is exposed to an micro-usb connector underneath a metal plate to the left go the SFP+ ports. To access it, we grab a micro-usb to USB-A cable and plug it into our computer.

### Finding the device

Once this is plugged in, you will need to find the device. On Mac or Linux you can list your devices and filter the output for one that contains serial in the name.

```
ls /dev/ | grep serial
cu.usbserial-0001
tty.usbserial-0001
```

On Windows you can look at device manager to see what new COM port is showing up.

### Connecting to the device

We need to connect with a baud rate of 115200 bits per second. On Mac or Linux, you can use the screen utility to connect to the device and then specify the rate of 115200 after the device name

```
screen /dev/cu.usbserial-0001 115200 
```

On Windows you can use putty, choose the Serial connection type and the specify the COM port and 115200 for the speed.

One interesting think to note is that the USB to serial adapter inside this box is powered over USB, so it will work even before you connect the device to power, allowing you to get output from the device from the earliest stages of power on.

### Power on

Plug power into the device and with any luck, you will get output like this:

```
BIOS Boot Selector for VEP1400-X
Version 3.50.0.9-10


POST Configuration
  CPU Signature 506F1
  CPU FamilyID=6, Model=5F, SteppingId=1, Processor=0
  Microcode Revision 2E
  Platform ID: 0x0
  PMG_CST_CFG_CTL: 0x37
  Misc EN: 0x840089
  Gen PM ConA: 0xA0800200
  Therm Status: 0x8000000
  POST Control=0xEA000F03, Status=0xE600DF00

BIOS initializations...

CPGC Memtest Channel 0 ...................... PASS
```

### Booting into Dell DiagOS

After POST you will get the option to enter the BIOS, press the delete key within 3 minutes to

```
Version 2.19.1266. Copyright (C) 2020 American Megatrends, Inc.
BIOS Date: 07/23/2020 10:56:50 Ver: 0ACHI040
Press <DEL> or <F2> to enter setup.    
```

After this you will find yourself in a very normal looking BIOS setup menu. The first time I experienced this, it was a bit surprising to me, being that we are connecting over a serial connection.

Once you enter the BIOS setup utility, use the arrows to move over to the "Save & Exit" tab, find and choose the USB drive under the "Boot Override" section before the hardware watchdog reboots the box!

With luck, you will get the prompt to choose what media to install DiagOS install on. We will install on the eMMC disk so that we can use DiagOS later even after we have installed Debian on the SSD.

```
  Booting `VEP1400 DiagOS Install'

                                                                                
Platform  : x86_64-dellemc_vep1400_c3538-r0                                     
Version   : 3.43.3.81-27                                                        
Build Date: 2022-12-08T01:14-08:00                                              
Info: Mounting kernel filesystems... done.                                      
starting to install vep1400 DiagOS                                              
discover: Rescue mode detected. No discover stopped.                            
ONIE: Executing installer: /diag-installer-x86_64-dellemc_vep1400_c3538-r0-3.43.3.81-27-2022-12-08.bin
Ignoring Verifying image checksum ... OK.                                       
cur_dir / archive_path /var/tmp/installer tmp_dir /tmp/tmp.1Ee4rd               
Preparing image archive ...sed -e '1,/^exit_marker$/d' /var/tmp/installer | tar xf -[   13.915879] sd 6:0:0:0: [sdb] No Caching mode page found
[   13.921345] sd 6:0:0:0: [sdb] Assuming drive cache: write through            
 OK.                                                                            
Diag-OS Installer: platform: x86_64-dellemc_vep1400_c3538-r0                    
platform found vep1400                                                          
platform vep1400 is supported.                                                  
console port ttyS0                                                              
                                                                                
****************************                                                    
Select Installation Device                                                      
****************************                                                    
1.SSD                                                                          
2.USB Disk
3.eMMC
0.Quit
---------------------------
Please select the device type that DIAG OS will be install on :                                  
```

![](//assets/VMwareedge620BIOS.jpg)Choose option 3 and hit enter. If you get errors similar to the below, don't worry. This happens because the label type of the partition on the eMMC is not of the type that Dia

```
Installing grub for diag-os
ERROR: grub-install failed on: /dev/mmcblk0
Installing for i386-pc platform.
grub-install: warning: this GPT partition label contains no BIOS Boot Partition; embedding won't be possible.
grub-install: warning: Embedding is not possible.  GRUB can only be installed in this setup by using blocklists.  However, blocklists are UNRELIABLE and their use is discouraged..
grub-install: error: will not proceed with blocklists.
Removing /tmp/tmp.ZkVRxp
Failure: Unable to install image: /diag-installer-x86_64-dellemc_vep1400_c3538-r0-3.43.3.81-27-2022-12-08.bin
This should be not reachable unless something wrong is there!!!!!
Info: BIOS mode: legacy
Info: Using eth0 MAC address: 18:5a:58:b3:3a:20
Info: eth0:  Checking link... down.
ONIE: eth0: link down.  Skipping configuration.
ONIE: Failed to configure eth0 interface
Starting: klogd... done.
Starting: dropbear ssh daemon... done.
Starting: telnetd... done.
discover: Rescue mode detected.  Installer disabled.

Please press Enter to activate this console. 
```

Press enter and then do the following to change the

Reset settings to defaults

Dell Diag OS on EMMC

Intel X553 vs I350