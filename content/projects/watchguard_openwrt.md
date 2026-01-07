+++
date = '2025-09-01T18:11:11-06:00'
draft = false
title = 'Porting OpenWRT to a WatchGuard Firebox T35'
slug = "watchguard_openwrt"
+++

## Status: Ongoing

### What's OpenWRT?
[OpenWRT](https://openwrt.org/#welcome_to_the_openwrt_project) is an open-source, Linux-based, community-developed, router OS for embedded devices. 

### What's a Firebox T35?
A Firebox T35 is a hardware firewall developed by WatchGuard. Its a 5-port, gigabyte firebox designed for small business and offices. 
It was declared end-of-life by WatchGuard in 2025, with support no longer being provided, nor security firmware/software updates. 
Because of it's end-of-life, it's no longer useful as a security appliance for production use. 

### Why then, install a router OS on a firewall?
One can find a glut of used, T35s on E-Bay for as little as $20 that are physically in perfect working order.
A friend of mine gave me her used T35 to tinker with. 
I was initially interested in installing OPNSense, an open-source firewall OS based on Open-BSD, on the hardware.
The T35, uses the NXP/Freescale T1024 SoC which uses a PowerPC64 architecture.
While the OpenBSD kernel and OPNSense supports PPC64 architecture, and even the QoriQ family of chips the T35 uses,
at the beginning of the project I was mistaken that it was not compatible with PPC64. 
Neither OpenWRT nor OPNSense support the T35 with readily installed image.

I decided that it was a worthwhile endeavor to contribute support for the T35 to the OpenWRT project.
Further, the T35 is part of the T-series, a series of similar tabletop firewalls, and development work on the T35 would likely
lead to easier development for support on the other devices in the series.
As of the time of writing, the only WatchGuard product with OpenWRT support is the m300.

### First steps
Gratefully, the T35 has a number of features that make it easier to reverse-engineer and develop on. 
Namely, the storage is a removable, writable mSATA SSD. 
None of the drive is encrypted (that I encountered, at least).
The first thing I did was remove the drive and capture an image of the entire SSD and each of the partitions.
On booting the WatchGuard, the user is greeted by a U-Boot boot-loader selection menu to boot into standard or recovery mode from the first three partitions.
The partitions each contain a kernel u-image and a device tree. 
As expected, the boot-loader boots the kernel in the selected partition.

There is a u-boot command line available where one could change the boot parameters and command-line, but it is password protected.
Ideally, I would reflash the boot-loader so that I could edit it, but a mistake would likely brick the board without a way for me to boot again.
Instead, my plan was to write the OpenWRT kernel and file-system to the SSD partitioned and named the same way such that the existing boot-loader would boot it none the wiser that it was actually booting OpenWRT instead.

### Getting a Linux command-line on the stock image
There was much information that would be useful the collect at runtime on the stock image of the T35, namely the information in the `/proc` file-system. The difficulty is, the shell that one gets in a T35, even when logged in as the admin user, is not a standard Linux shell where one could read and edit files; it's a secure network-appliance shell that deliberately makes it difficult to escape and execute standard Linux commands. 
My first attempt was the swap out the `/sbin/init` process so my script would collect the information I needed and dump it to a file on the disk. 
The two problems were: 

1) Without the original `init` process running, much of the info I needed was uninitialized... whoda guessed that?
2) The file-system was a `tempfs` so all writes, even to an external USB drive, were ephemeral

Back to the drawing board...

Fortunately, the WatchGuard engineers sensibly named the user the CLI process runs as user `cli` and the shell it runs is `/usr/bin/cli`.
I swapped the file `/usr/bin/cli` with a BusyBox shell executable, and after logging in through the normal WatchGuard log-in, it launched me a BusyBox shell where I could read and list the files I needed from the `/proc` tree.
The WatchGuard stock kernel left `CONFIG_IKCONFIG_PROC=y` in their `.config`, so I was able to get the original kernel configuration from the running, original stock image.

