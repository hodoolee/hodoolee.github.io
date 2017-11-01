---
layout: post
title: Computer network and IP address
comments: true
tags:
- Computer Networks
---

> IP address is an address having information about how to reach a specific host, especially outside the LAN. An IP address is a 32 bit unique address having an address space of 2^32. Generally, there are two notations in which IP address is written, dotted decimal notation and hexadecimal notation.

Let's say there are two networks, A and B. If A wants to send a request to B, then A needs to know the network ID(NID), the host ID(HID) and the port number of B. Thankfully, IP address has both NID and HID so if we type ```www.google.com```, the ```ISP``` converts it into an IP address using ```DNS```. This step is called **DNS overhead** and it saves the IP address locally at the first time and re-uses until it expires. All the web services are pre-defined and fixed at port number 80 which is globally known.

In 32-bits system, the total number of IP addresses is 2^32. Back in 1970s and 1980s, 256 networks were enough to use. However, recent days need more networks.

32-bits |
--- | ---
8-bits | 24-bits
256 | 16M

## **Classful IP addressing**
- Class A
- Class B
- Class C
- Class D
- Class E
