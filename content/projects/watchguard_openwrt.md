+++
date = '2025-09-01T18:11:11-06:00'
draft = false
title = 'Porting OpenWRT to a WatchGuard Firebox T35'
+++

## Status: Ongoing

### What's OpenWRT?
[OpenWRT](https://openwrt.org/#welcome_to_the_openwrt_project) is an open-source, linux-based, community-developed, router OS for embedded devices. 

### What's a Firebox T35?
A Firebox T35 is a hardware firewall developed by WatchGuard. Its a 5-port, gigabyte firebox designed for small buisness and offices. 
It was declared end-of-life by WatchGuard in 2025, with support no longer being provided, nor security firmware/software updates. 
Becasue of it's end-of-life, it's no longer useful as a security applicance for production use. 

### Why then, install a router OS on a firewall?
One can find a glut of used, T35s on Ebay for as little as $20 that are physically in perfect working order.
A friend of mine gave me her used T35 to tinker with. 
I was initially interested in installing OPNSense, an open-source firewall OS based on Open-BSD, on the hardware.
The T35, uses the NXP/Freescale T1024 SoC which uses a PowerPC64 architecture.
While the OpenBSD kernel and OPNSense supports PPC64 architecture, and even the QoriQ family of chips the T35 uses,
at the begining of the project I was mistaken that it was not compatible with PPC64. 
Neither OpenWRT nor OPNSense support the T35 with readily installed image.

I decided that it was a worthwhile endeavour to contribute support for the T35 to the OpenWRT project.
Further, the T35 is part of the T-series, a series of similar tabletop firewalls, and development work on the T35 would likely
lead to easier development for support on the other devices in the series.
As of the time of writing, the only WatchGuard product with OpenWRT support is the m300.

### First steps
Gratefully, the T35 has a number of features that make it easier to reverse engineer and develop on. 
Namely, the storage is a removable, writable mSATA SSD.

