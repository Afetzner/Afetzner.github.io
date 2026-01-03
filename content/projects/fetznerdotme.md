+++
date = '2026-01-01T20:43:30-06:00'
draft = false
title = 'This website!'
slug = "fetznerdotme"
+++
## You're looking at it! 

### Right now!

#### Innit neat?!

As you cas see at the footer, it's made with [Hugo](https://gohugo.io/) and uses a theme [Hermit-V2](https://github.com/1bl4z3r/hermit-V2). 
I'm not much of a front-end web developer, so I gravitate toward tools that let me focus on writing content in Markdown and generate static HTML/CSS as output.
Because of that, this page isn’t really about front-end design---it’s about how the site is hosted and the principles behind it.

Since the website is self-hosted, my primary objectives are:
1) Minimizing attack area
2) Isolating the web-server should it get compromised

I'm trepid about giving away too many details about my security posture, but I feel confident enough in my stack that I can disclose the following:

### Minimizing attack area
The website is intentionaly simple. 
It's a statically served HTML/CSS/JavaScript site with no server-side application logic or database. 
This significanlty reduces the complexity and atack surface compared to a dynamic website.

Public traffic is mangaged by a reverse proxy that
- Hides my IP address
- provides some protection against denial-of-service and other network attacks
- minimizes the traffic I have to allow from the internet into my nework

As such, the public facing part of my server does as little as possible and only as much as neccessary.

### Isolating and containing the server
The entire stack uses the principal of least privilege:
- The web server runs in a rootless container of an unprivilaged user.
- It has only read-only access to only the files it needs.
- It also runs with OS-level mandatory access controls.

Further, the server itself is isolated from the rest of my network using VLAN-segregation and 
a firewall enforces minimal access to the WAN using egress filtering, and zero access to the LAN.

> My design philosophy is to assume a breach will happen and minimize its impact when it does.
