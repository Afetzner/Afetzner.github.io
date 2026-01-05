+++
date = '2025-10-01T20:16:51-06:00'
draft = false
title = "OPNSense Firewall"
slug = "opnsense_firewall"
+++

## Status: Ongoing

### What's OPNSense?
[OPNSense](https://opnsense.org/#) is an open-source, Free BSD-based, firewall OS.

You might've picked up that this is similar to [my OpenWRT/T35 project]({{< ref "watchguard_openwrt" >}}).
In fact, the original intention was to use the T35 as a firewall as is it originally was using OPNSense. 
I was discouraged by the fact that OPNSense uses FreeBSD, and although it is Unix-like, I am less familiar with BSD compared to Linux.
On hitting a roadblock getting the internal network-switch to work on my T35 using OpenWRT, I decided to bite the bullet and buy a OPNSense-suitable appliance.
I bought a Dell Wyse with two NICs and installed OPNSense onto it.

OPNSense has richer firewall rules than OpenWRT, and a larger computational footprint. 
In summary, OpenWRT is for embedded routers whereas OPNSense is a full-fledged firewall.

Before I had my OPNSense firewall, I had a Raspberry Pi 4 running IPFire. 
I was underwhelmed by the features and limitations of IPFire, but principally, the RPi 4 has only one network port and an SD card for storage. 
I was bottlenecking my network by using a USB-Ethernet adapter for one of the interfaces. 
Also, IPFire has a limitation that it can only have one VLAN assigned per interface, which disrupted my plans to segregate my LAN, WLAN, DMZ, and work VLANS on the same interface.


### What I am doing with OPNSense
I host a server (including [this website]({{< ref "fetznerdotme" >}})) and I wanted to segregate traffic to and from my server from the rest of my network. 
This is accomplishable with VLAN segregation and the NetGear VLAN-aware switch I have. It also lets me segregate and monitor my work VLAN and WLAN's traffic.

OPNSense has intrusion detection and prevention baked into it, which I've yet to fully explore, but I utilize it.

I also wanted to host a [wireguard](https://www.wireguard.com/) peer to serve as a relay and ingress for my private LAN. 
The WireGuard tunnel lets me stream video, VNC, file-sharing, and ssh from and between my trusted devices and my LAN. 

Lastly, OPNSense can host a DNS server, which was the original catalyst for using IPFire. That way I can address my servers, PC, laptop, and phone by domain name instead of IP; very handy!
