---
layout: post
title: Types of Casting
comments: true
tags:
- Computer Networks
---

If the IP address is given, ```11.0.0.0```, we know that it is a class A network. Also, ```11``` is NID and ```0.0.0``` is HID and since HID is all zero, it is used to represent the entire network.

## **Unicast**
It is communication between a single sender and a single receiver over a network. Therefore, sending a message from the host to the other is called **unicasting**.

Data | Source Address | Destination Address

## **Broadcast**

- Limited Broadcast

If a packet is sent to the source address, then it sends the packet to all the hosts on the local network.

Data | Source Address | Destination Address
:---: | :---: | :---:
message | 11.1.2.3 | 255.255.255.255

- Directed Broadcast

There are two different networks, ```11.0.0.0``` and ```20.0.0.0```. If ```11.1.2.3``` sends a traffic to all hosts in network ```20.0.0.0```, then it is called **Directed Broadcasting**

Data | Source Address | Destination Address
:---: | :---: | :---:
message | 11.1.2.3 | 20.255.255.255

In class A network, although 2<sup>24</sup> IP addresses are present per network, we are going to have 2<sup>24</sup> - 2 hosts are available per network only. The reason is that two hosts are used for network ID and directed broadcast address.

Class | # IP available | # host available
:---: | :---: | :---:
A | 2<sup>24</sup> | 2<sup>24</sup> - 2
B | 2<sup>16</sup> | 2<sup>16</sup> - 2
C | 2<sup>8</sup> | 2<sup>8</sup> - 2

## **Example**

Class A: 1 - 126  
Class B: 128 - 191  
Class C: 192 - 223  

IP | NID | DBA | LBA
:---:| :---: | :---: | :---:
1.2.3.4 | 1.0.0.0 | 1.255.255.255 | 255.255.255.255
130.1.2.3 | 130.1.0.0 | 130.255.255.255 | 255.255.255.255
200.1.10.100 | 200.1.10.0 | 200.1.10.255 | 255.255.255.255
250.0.1.2 | X | X | X

Class D and E have no NID, HID, DBA, and LBA. Since ```250.0.1.2``` falls in class E so it does not have any of them.

## **References**
- [Types of Casting: Unicast,Limited Broadcast,Directed Broadcast](https://www.youtube.com/watch?v=hffYt7RDrgk)
