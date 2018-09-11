---
title: 'Computer Networking, Chapter 1, Computer Networks and the Internet, Notes'
comments: true
categories: Notes
tags:
  - Computer Networking
abbrlink: 527c28
date: 2018-09-04 10:13:08
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
DSLAM: DSL access multiplexer
![DSL Internet access](/img/cn1.1.png)


Fiber optics connect the cable head end to neighborhood-level junctions, from which traditional coaxial cable is then used to reach individual houses and apartments.
![A hybrid fiber-coaxial access network](/img/cn1.2.png)
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
   high-speedoperation, low error rate
4. Terrestrial Radio Channels
5. Satellite Radio Channels
   geostationary satellites, low-earth orbiting (LEO) satellites

# 1.3 Network Core

## 1.3.1 Packet Switching
packet switches (for which there are two predominant types, routers and link layer switches)

### Store-and-Forward Transmission
> The packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.

### Queuing Delays and Packet Loss
**output buffer**
In addition to the store-and-forward delays, packets suffer output buffer **queuing delays**.

**Packet loss** may occur when buffer is completely full.

### Forwarding Tables and Routing Protocols
Each router has a **forwarding table** that maps destination addresses (or portions of the destination addresses) to that router’s outbound links. 

The Internet has a number of special **routing protocols** that are used to automatically set the forwarding tables.

### Ad. & Disad.
1. great for bursty data (resource sharing, simpler)
2. excessive congestion possible.

## 1.3.2 Circuit Switching
In circuit-switched networks, the resources needed along a path (buffers, link transmission rate) to provide for communication between the end systems are reserved for the duration of the communication session between the end systems.

e.g. traditional telephone networks

**guaranteed** constant rate

### Multiplexing in Circuit-Switched Networks
A circuit in a link is implemented with either frequency-division multiplexing (FDM) or time-division multiplexing (TDM).

频分/时分多路复用

## 1.3.3 A Network of Networks

1. regional ISPs
2. tier-1 ISPs
3. **PoP**: points of presence, a group of one or more routers (at the same location) in the provider’s network where customer ISPs can connect into the provider ISP.
4. Any ISP (except for tier-1 ISPs) may choose to **multi-home**, that is, to connect to two or more provider ISPs. 
5. ISPs at the same level of the hierarchy can **peer**. When two ISPs peer, it is typically settlement-free, that is, neither ISP pays the other.
6. **Internet Exchange Point (IXP)**: a meeting point where multiple ISPs can peer together. 
7. **content provider networks**

见P61 图片

# 1.4 Delay, Loss, and Throughput in Packet-Switched Networks
## 1.4.1 overview
### Processing Delay
The time required to:
1. examine the packet’s header and determine where to direct the packet 
2. check for bit-level errors in the packet
(on the order of microseconds)
### Queuing Delay
The number of packets that an arriving packet might expect to find is a function of the intensity and nature of the traffic arriving at the queue.
### Transmission Delay
L/R
### Propagation Delay
d/s

## 1.4.2 Queuing Delay and Packet Loss
traffic intensity: La/R (a: the average rate at which packets arrive at the queue, in units of packets/sec)
Golden rule: Design your system so that the traffic intensity is no greater than 1.
As the traffic intensity approaches 1, the average queuing delay increases rapidly.

### Packet Loss

## 1.4.3 End-to-End Delay
Traceroute
## 1.4.4 Throughput in Computer Networks
When transferring a large file from Host A to Host B, the **instantaneous throughput** at any instant of time is the rate (in bits/sec) at which Host B is receiving the file. 

**Average throughput**: F/T bits/sec



