---
title: 'Computer Networking, Chapter 1, Computer Networks and the Internet, Notes'
comments: true
date: 2018-09-04 10:13:08
categories: Notes
tags: 
  - Computer Networking

---

## 1.1.3 What's Protocol?
A protocol defines the format and the order of messages exchanged between two or more communicating entities, as well as the actions taken on the transmission and/or receipt of a message or other event.

# 1.2 Network Edge
**end systems** = **hosts** (Two categories: clients and servers)

Today, most of the servers from which we receive search results, e-mail, Web pages, and videos reside in large **data centers**.

## 1.2.1 Access Networks
The **access network**: the network that physically connects an end system to the first router (also known as the “edge router”) on a path from the end system to any other distant end system. 

### Home Access
Today, the two most prevalent types of broadband residential access are **digital subscriber line** (DSL) and **cable**.

DSL: 数字用户线路，是以电话线为传输介质的传输技术组合。
![DSL Internet access](img/cn1.1.png)


Fiber optics connect the cable head end to neighborhood-level junctions, from which traditional coaxial cable is then used to reach individual houses and apartments.
![A hybrid fiber-coaxial access network](img/cn1.2.png)
Because both fiber and coaxial cable are employed in this system, it is often referred to as **hybrid fiber coax** (HFC).

Coaxial cable: 同轴电缆
cable modem termination system (CMTS)

### Ethernet and Wi-Fi
LAN: local area network
Although there are many types of LAN technologies, Ethernet is by far the most prevalent access technology.

### Wide-Area Wireless Access: 3G and LTE
LTE: Long-Term Evolution 长期演进技术

## 1.2.2 Physical Media
Physical media fall into two categories: guided media and unguided media.

With guided media, the waves are guided along a solid medium, such as a fiber-optic cable, a twisted-pair copper wire, or a coaxial cable. With unguided media, the waves propagate in the atmosphere and in outer space, such as in a wireless LAN or a digital satellite channel.

1. Twisted-Pair Copper Wire
2. Coaxial Cable
3. Fiber Optics
4. Terrestrial Radio Channels
5. Satellite Radio Channels
   geostationary satellites, low-earth orbiting (LEO) satellites

# 1.3 Network Core

## 1.3.1 Packet Switching
packet switches (for which there are two predominant types, routers and linklayer switches)

### Store-and-Forward Transmission
> The packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.



