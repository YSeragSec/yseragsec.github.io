--- 
title: "Networking Fundamentals — Week 1: OSI Model, HTTP, DNS & Transport Layer"
date: 2025-01-01 00:00:00 +0200
categories: [CatReloaded Entry Level, Networking]
tags: [networking, OSI, HTTP, DNS, TCP, UDP, IP, CCNA, fundamentals]
description: My Week 1 notes from CAT Reloaded entry-level program covering the OSI model layers, DNS, HTTP, TCP/UDP, IP addressing, and subnetting.
---

> These are my personal study notes from the 4-week CAT Reloaded entry-level program. Week 1 focused on networking fundamentals.

---

## Network

### Open System Interconnection (OSI) Model

<p style="font-size: 1.15em; font-style: italic;">The OSI model is an international standard that serves as a basis for how technology vendors can describe the role of their technologies in enabling interoperability and communication and how they interact with other components.</p>



### Layers of OSI

#### How Data Flows in the OSI Model?

<p style="font-size: 1.15em;">When we transfer information from one device to another, it travels through 7 layers of the OSI model. First data travels down through 7 layers from the sender's end and then climbs back 7 layers on the receiver's end. Data flows through the OSI model in a step-by-step process:</p>

- **Application Layer:** Applications create the data.
- **Presentation Layer:** Data is formatted and encrypted.
- **Session Layer:** Connections are established and managed.
- **Transport Layer:** Data is broken into segments for reliable delivery.
- **Network Layer:** Segments are packaged into packets and routed.
- **Data Link Layer:** Packets are framed and sent to the next device.
- **Physical Layer:** Frames are converted into bits and transmitted physically.
