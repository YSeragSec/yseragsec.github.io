---
title: "Networking: OSI Model, HTTP, DNS, TCP/UDP & More"
date: 2026-02-13 00:00:00 +0200
categories: [Infosec Field Notes, Networking]
tags: ["cat entry level", networking, OSI, HTTP, DNS, TCP, UDP, IP, subnetting, DHCP, NAT, routing, fundamentals]
description: My Week 1 notes from the CAT Reloaded entry-level program. Covers the OSI model layers, DNS, HTTP, TCP/UDP, IP addressing, subnetting, DHCP, NAT, and routing.
last_modified_at: 2026-02-1 00:00:00 +0200
image:
  path: assets/img/posts/CATEntryLevel/Networking/ASCII - Earth is a space station!.jpg
---

<p style="font-size: 1.15em;">These are my personal study notes from the 4-week <b>CAT Reloaded</b> entry-level program. Week 1 focused on networking fundamentals — the foundation every security engineer needs to have solid before anything else.</p>

---

## Network

### Open System Interconnection (OSI) Model

<p style="font-size: 1.15em; font-style: italic;">The OSI model is an international standard that serves as a basis for how technology vendors can describe the role of their technologies in enabling interoperability and communication and how they interact with other components.</p>

---

### Layers of OSI

#### How Data Flows in the OSI Model?

<p style="font-size: 1.15em;">When we transfer information from one device to another, it travels through 7 layers of the OSI model. First data travels down through 7 layers from the sender's end and then climbs back 7 layers on the receiver's end:</p>

- **Application Layer:** Applications create the data.
- **Presentation Layer:** Data is formatted and encrypted.
- **Session Layer:** Connections are established and managed.
- **Transport Layer:** Data is broken into segments for reliable delivery.
- **Network Layer:** Segments are packaged into packets and routed.
- **Data Link Layer:** Packets are framed and sent to the next device.
- **Physical Layer:** Frames are converted into bits and transmitted physically.

| Layer | Name | Data Unit | Key Device / Protocol |
|---|---|---|---|
| 7 | Application | Data | HTTP, DNS, FTP |
| 6 | Presentation | Data | SSL/TLS, encoding |
| 5 | Session | Data | NetBIOS, RPC |
| 4 | Transport | Segment | TCP, UDP |
| 3 | Network | Packet | IP, Router |
| 2 | Data Link | Frame | MAC, Switch |
| 1 | Physical | Bits | Cables, NIC |

---

## Upper Layers (7 → 5)

### Layer 7 — Application Layer — DNS & HTTP

<p style="font-size: 1.15em;">The Application Layer serves as the interface between end-user applications and the underlying network services. It provides protocols and services that allow applications to communicate across the network. Key functionalities include resource sharing, remote file access, and network management.</p>

<p style="font-size: 1.15em;"><b>Key protocols:</b> HTTP (web browsing), FTP (file transfers), SMTP (email), DNS (domain name resolution)</p>

---

### The Domain Name System (DNS)

<p style="font-size: 1.15em;">The Domain Name System (DNS) is a hierarchical and distributed naming system that translates human-readable domain names into IP addresses, enabling users to access websites easily.</p>

#### How DNS Works

1. **User Input** — You type `www.geeksforgeeks.org` into your browser
2. **Local Cache Check** — Browser checks if it has recently resolved this domain
3. **DNS Resolver Query** — If not cached, your computer asks the DNS Resolver (usually from your ISP)
4. **Root DNS Server** — Resolver asks a Root Server, which directs it to the correct TLD server (e.g. `.org`)
5. **TLD Server** — Directs the resolver to the Authoritative DNS Server for `geeksforgeeks.org`
6. **Authoritative DNS Server** — Holds the actual IP address and returns it to the resolver
7. **Final Response** — Resolver sends the IP to your computer, browser connects and loads the page

#### DNS Hierarchy

```
Root
 └── TLDs (.com, .org, .net, .edu)
      └── Second-Level Domains (example, website)
           └── Subdomains (www, mail, blog)
                └── Hostnames (web1, mailserver, ftp)
```

---

### Hypertext Transfer Protocol (HTTP)

<p style="font-size: 1.15em;">HTTP is a <b>stateless</b> protocol — it doesn't remember previous requests. Websites use <b>cookies</b> to work around this and remember user sessions.</p>

#### HTTP Requests

<p style="font-size: 1.15em;">HTTP Requests are messages sent by the client to request data or perform actions on the server:</p>

- **GET** — Read/retrieve data from the server → returns `200 OK`
- **POST** — Send data (file, form, etc.) to the server → returns `201 Created`
- **PUT** — Replace entire content at a location. Creates the resource if it doesn't exist
- **PATCH** — Modify only part of the data at a location
- **DELETE** — Delete data at a specified location

#### Common Request Headers

| Header | Purpose |
|---|---|
| `Host` | Tells the server which website you want (for multi-site servers) |
| `User-Agent` | Browser name and version for proper formatting |
| `Content-Length` | How much data is being sent so the server doesn't miss any |
| `Accept-Encoding` | Compression methods the browser supports |
| `Cookie` | Stored data sent back to the server on each request |

#### Common Response Headers

| Header | Purpose |
|---|---|
| `Set-Cookie` | Data to store and send back on future requests |
| `Cache-Control` | How long to cache the response before re-requesting |
| `Content-Type` | Type of data returned (HTML, CSS, JSON, image, etc.) |
| `Content-Encoding` | Compression method used on the response |

#### HTTP Status Codes

| Range | Meaning |
|---|---|
| `100-199` | Informational — request received, continue |
| `200-299` | Success |
| `300-399` | Redirection |
| `400-499` | Client Errors |
| `500-599` | Server Errors |

**Common codes:**

| Code | Meaning |
|---|---|
| `200` | OK |
| `201` | Created |
| `301` | Moved Permanently |
| `302` | Found (Temporary Redirect) |
| `400` | Bad Request |
| `401` | Not Authorised |
| `403` | Forbidden |
| `404` | Page Not Found |
| `405` | Method Not Allowed |
| `500` | Internal Server Error |
| `503` | Service Unavailable |

---

### Layer 6 — Presentation Layer

<p style="font-size: 1.15em;">Also known as the <b>syntax layer</b>. Responsible for translating data between the application layer and the network format, ensuring that data from one system is readable by another.</p>

**Functions:**
- **Translation** — e.g. ASCII to EBCDIC
- **Encryption/Decryption** — Converts data to ciphertext (encrypted) and back to plain text using a key
- **Compression** — Reduces the number of bits transmitted over the network

---

### Layer 5 — Session Layer

<p style="font-size: 1.15em;">Responsible for the establishment, management, and termination of sessions between two devices. Also provides authentication and security.</p>

**Functions:**
- **Session Establishment, Maintenance, and Termination** — Opens, uses, and closes connections
- **Synchronization** — Adds checkpoints to data so transmission can resume from the last checkpoint on failure
- **Dialog Controller** — Manages half-duplex or full-duplex communication

**Example:** When you send a message on a browser-based messenger, the session layer establishes the connection, the presentation layer compresses and encrypts the message, and it gets converted to bits for transmission.

---

## Heart of OSI — Layer 4

### Layer 4 — Transport Layer — TCP & UDP

<p style="font-size: 1.15em;">Provides services to the application layer and takes services from the network layer. Data at this layer is called a <b>Segment</b>. It is responsible for <b>end-to-end delivery</b> of the complete message.</p>

- Provides acknowledgment of successful transmission and retransmits on error
- Protocols: **TCP**, **UDP**, NetBIOS, PPTP
- Adds Source and Destination **port numbers** in the header
- **Example:** Web applications use port `80` by default (HTTP), `443` for HTTPS

**Functions:**
- **Segmentation and Reassembly** — Breaks messages into segments, reassembles at destination
- **Service Point Addressing** — Port addresses ensure data reaches the correct process/application

#### Ports

| Range | Purpose |
|---|---|
| `1 – 1023` | Well-known ports (HTTP, HTTPS, DNS, SMTP, SSH, FTP, Telnet) |
| `1024 – 49151` | Available ports for use |
| `49152 – 65535` | Reserved by the OS for outgoing connections |

**Important ports to know:**

| Port | Protocol |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP |
| 443 | HTTPS (SSL/TLS) |
| 3389 | RDP |

---

### TCP — Transmission Control Protocol

<p style="font-size: 1.15em;">TCP is the most used protocol. It performs many functions to ensure data validation and a reliable connection:</p>

- Detects lost or failed data and retransmits it
- Filters duplicate data
- Designed for **accurate delivery**, not speed

**TCP Flags:** SYN, ACK, FIN, Push, Reset — pieces of information in the TCP header to help ensure accurate delivery.

#### Three-Way Handshake

1. **SYN** — Client sends SYN to request a connection
2. **SYN-ACK** — Server acknowledges and agrees to the connection
3. **ACK** — Client confirms, connection is established

---

### UDP — User Datagram Protocol

<p style="font-size: 1.15em;">UDP operates at the same level as TCP but is <b>connectionless and stateless</b>:</p>

- No handshake
- No failure packet detection
- No retransmission

<p style="font-size: 1.15em;">Because of this, UDP is <b>faster than TCP</b> and is used where accuracy is less important than speed — e.g. audio/video streaming where one lost packet doesn't significantly affect the experience.</p>

---

## Media Layers (3 → 2)

### Layer 3 — Network Layer — Router

<p style="font-size: 1.15em;">Works for the transmission of data from one host to another located in <b>different networks</b>. Handles packet routing — selecting the shortest path from the available routes. Data at this layer is called a <b>Packet</b>. Implemented by routers and switches.</p>

**Functions:**
- **Routing** — Determines the best path from source to destination
- **Logical Addressing** — Places sender and receiver IP addresses in the packet header

---

### IPv4

<p style="font-size: 1.15em;">A 32-bit address divided into 4 octets (e.g. <code>192.168.1.1</code>). IPv4 is divided into classes:</p>

| Class | Range | Example | Notes |
|---|---|---|---|
| A | 0-127 | `0.0.0.0 – 127.0.0.0` | First 8 bits = network |
| B | 128-191 | `128.0.0.0 – 191.255.0.0` | First 16 bits = network |
| C | 192-223 | `192.0.0.0 – 223.255.255.0` | First 24 bits = network |
| D | 224-239 | `224.0.0.0 – 239.255.255.255` | Multicast |
| E | 240-255 | `240.0.0.0 – 255.255.255.255` | Experimental |

**Reserved Addresses:**

| Range | Purpose |
|---|---|
| `127.0.0.0/8` | Localhost (loopback) |
| `10.0.0.0/8` | Private LAN (Class A) |
| `172.16.0.0 – 172.31.255.255` | Private LAN (Class B) |
| `192.168.0.0/16` | Private LAN (Class C) |

> **Note:** First IP of any network = Network Identifier. Last IP = Broadcast Address.
{: .prompt-info }

**Private Network IDs and Broadcast Addresses:**

| Class | Network ID | Broadcast Address |
|---|---|---|
| A | `10.0.0.0` | `10.255.255.255` |
| B | `172.16.0.0` | `172.16.255.255` |
| C | `192.168.1.0` | `192.168.1.255` |

---

### Subnet Mask & CIDR

<p style="font-size: 1.15em;"><b>Subnet Mask</b> — defines which part of an IP address belongs to the network and which belongs to the host device.</p>

**Default subnet masks by class:**

| Class | Default Subnet Mask |
|---|---|
| A | `255.0.0.0` |
| B | `255.255.0.0` |
| C | `255.255.255.0` |

<p style="font-size: 1.15em;"><b>CIDR (Classless Inter-Domain Routing)</b> — a shorthand way to write the subnet mask size using bit count instead of full numbers:</p>

| Class | Example | CIDR | Equivalent Mask |
|---|---|---|---|
| A | `10.0.0.0` | `/8` | `255.0.0.0` |
| B | `172.16.0.0` | `/16` | `255.255.0.0` |
| C | `192.168.1.0` | `/24` | `255.255.255.0` |

---

### Default Gateway

<p style="font-size: 1.15em;">The Default Gateway is responsible for routing traffic from one network to another. In most cases it is the <b>first usable IP</b> in the network:</p>

- Class C: `192.168.1.1`
- Class B: `172.16.0.1`
- Class A: `10.0.0.1`

---

### DHCP — Dynamic Host Configuration Protocol

<p style="font-size: 1.15em;">A server that automatically assigns IP configuration to devices on a network. In a home network, the DHCP server is built into the router. In enterprise networks it's a separate server.</p>

**DHCP automatically assigns:**
- IP Address (e.g. `192.168.1.10`)
- Subnet Mask (e.g. `255.255.255.0`)
- Default Gateway (e.g. `192.168.1.1`)
- DNS Server (e.g. `8.8.8.8`)

---

### Routing & NAT

#### Routing Table

<p style="font-size: 1.15em;">A table in the router that determines the next hop to reach a destination. If no specific route exists, the packet is sent to the <b>default gateway</b>. If multiple routes exist, the router chooses the shortest one.</p>

**Routing Protocols determine:**
- Next hop
- Shortest path
- Network changes
- Link failures

**Protocol types:**
- **RIP** — Broadcasts routing table every 30 seconds, determines shortest path
- **OSPF** — Detects network topology changes and link failures
- **BGP** — Most widely used, can determine shortest path and reroute on failure

#### NAT — Network Address Translation

<p style="font-size: 1.15em;">NAT is a technique for translating one IP address to one or more IP addresses. All home networks (LANs) use NAT — multiple private devices share a single public IP when communicating with the internet.</p>

#### ICMP — Internet Control Message Protocol

<p style="font-size: 1.15em;">Used to test connectivity between two hosts. The most common use is <code>ping</code>.</p>

- Sends an **ICMP Echo Request** to the target
- Target replies with an **ICMP Echo Reply**

**Traceroute** uses ICMP with incrementing TTL (Time To Live) values to map each hop along the route to a destination.

---

### Layer 2 — Data Link Layer — Switch

<p style="font-size: 1.15em;">Responsible for <b>node-to-node</b> data transfer and error detection and correction. Ensures data is transmitted to the correct device on a local network segment. Data at this layer is called a <b>Frame</b>. Implemented by switches and bridges.</p>

- When a packet arrives, the DLL transmits it to the correct host using its **MAC address**
- DLL encapsulates sender and receiver MAC addresses in the frame header
- Receiver's MAC is found using **ARP** — broadcasts "Who has this IP?" and the target replies with its MAC

**Sublayers:**
- **LLC** (Logical Link Control)
- **MAC** (Media Access Control)

**Functions:**
- **Framing** — Attaches special bit patterns to the start and end of each frame
- **Physical Addressing** — Adds MAC addresses to the frame header
- **Error Control** — Detects and retransmits damaged or lost frames
- **Flow Control** — Coordinates data rate so the receiver isn't overwhelmed
- **Access Control** — Determines which device controls the shared channel at any given time

> The **CAM Table** in a switch maps MAC addresses to physical ports — this is how the switch knows where to forward each frame.
{: .prompt-tip }

---

## Hardware Layer — Layer 1

### Layer 1 — Physical Layer — Raw

<p style="font-size: 1.15em;">Responsible for the <b>physical connection</b> between devices. Defines the hardware elements involved — cables, switches, and other physical components. Also specifies the electrical, optical, and radio characteristics of the network.</p>

**Functions:**
- **Bit Synchronization** — A clock controls both sender and receiver to keep them in sync at the bit level
- **Bit Rate Control** — Defines the transmission rate (bits per second)
- **Physical Topologies** — How devices are arranged: bus, star, or mesh topology
- **Transmission Mode** — How data flows between connected devices: simplex, half-duplex, or full-duplex

---

***If you have any questions or comments, feel free to reach out on [LinkedIn](https://www.linkedin.com/in/yousefserag/) or [Discord](https://discord.com/users/751180791492378774)***
