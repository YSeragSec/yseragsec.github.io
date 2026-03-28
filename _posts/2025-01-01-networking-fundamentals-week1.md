--- 
title: "Networking Fundamentals — Week 1: OSI Model, HTTP, DNS & Transport Layer"
date: 2025-01-01 00:00:00 +0200
categories: [CatReloaded Entry Level, Networking]
tags: [networking, OSI, HTTP, DNS, TCP, UDP, IP, CCNA, fundamentals]
description: My Week 1 notes from CAT Reloaded entry-level program covering the OSI model layers, DNS, HTTP, TCP/UDP, IP addressing, and subnetting.
---

> These are my personal study notes from the 4-week CAT Reloaded entry-level program. Week 1 focused on networking fundamentals.

---

## The OSI Model

The OSI (Open System Interconnection) model is an international standard that describes how technology components communicate and interact across a network. Data travels **down** through 7 layers on the sender's side, and **back up** through 7 layers on the receiver's side.

![OSI Model Animation](https://media.geeksforgeeks.org/wp-content/uploads/20241111182857579134/OSI-Model.gif)

---

## Upper Layers (7 → 5)

### Layer 7 — Application Layer

The interface between end-user applications and the underlying network. Handles resource sharing, remote file access, and network management.

**Key protocols:** HTTP, FTP, SMTP, DNS

---

### DNS — Domain Name System

DNS translates human-readable domain names into IP addresses.

**How DNS resolution works:**

1. You type `www.example.com` into your browser
2. Browser checks its **local cache** first
3. If not cached → queries your **DNS Resolver** (usually from your ISP)
4. Resolver queries a **Root DNS Server** → gets directed to the correct TLD server (e.g. `.com`)
5. TLD server points to the **Authoritative DNS Server** for the domain
6. Authoritative server returns the actual IP address
7. Your browser connects to that IP and loads the page

---

### HTTP — Hypertext Transfer Protocol

#### HTTP Methods

| Method | Purpose |
|---|---|
| `GET` | Retrieve data from server → returns `200 OK` |
| `POST` | Send data to server → returns `201 Created` |
| `PUT` | Replace entire resource at a location |
| `PATCH` | Partially update a resource |
| `DELETE` | Delete a resource |

#### Common Request Headers

| Header | Purpose |
|---|---|
| `Host` | Specifies which website on a multi-site server |
| `User-Agent` | Browser info so server formats response correctly |
| `Content-Length` | How much data is being sent |
| `Accept-Encoding` | Compression methods the browser supports |
| `Cookie` | Stored data sent back to the server |

#### Common Response Headers

| Header | Purpose |
|---|---|
| `Set-Cookie` | Stores info to be sent back on future requests |
| `Cache-Control` | How long to cache the response |
| `Content-Type` | Type of data returned (HTML, JSON, image, etc.) |
| `Content-Encoding` | Compression method used |

#### HTTP Status Codes

| Range | Meaning |
|---|---|
| `100-199` | Informational |
| `200-299` | Success |
| `300-399` | Redirection |
| `400-499` | Client Errors |
| `500-599` | Server Errors |

**Common ones to know:**

| Code | Meaning |
|---|---|
| `200` | OK |
| `201` | Created |
| `301` | Moved Permanently |
| `302` | Found (Temporary Redirect) |
| `400` | Bad Request |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Not Found |
| `405` | Method Not Allowed |
| `500` | Internal Server Error |
| `503` | Service Unavailable |

---

### Layer 6 — Presentation Layer

Translates data between the application layer and the network format. Also called the **syntax layer**.

**Functions:**
- **Translation** — e.g. ASCII to EBCDIC
- **Encryption/Decryption** — converts data to ciphertext and back
- **Compression** — reduces bits transmitted over the network

---

### Layer 5 — Session Layer

Manages the establishment, maintenance, and termination of sessions between two devices.

**Functions:**
- **Session Management** — open, use, close connections
- **Synchronization** — adds checkpoints to data to recover from errors
- **Dialog Control** — manages half-duplex or full-duplex communication

---

## Heart of OSI (Layer 4)

### Layer 4 — Transport Layer (TCP & UDP)

Responsible for **end-to-end delivery** of the complete message. Data at this layer is called a **Segment**.

**Functions:**
- **Segmentation & Reassembly** — breaks messages into segments, reassembles at destination
- **Port Addressing** — ensures data reaches the correct process/application
- **Flow & Error Control** — ensures reliable transmission

---

### TCP — Transmission Control Protocol

Connection-oriented, reliable, ordered delivery.

**Three-Way Handshake:**

1. **SYN** — Client sends SYN to request connection
2. **SYN-ACK** — Server acknowledges and agrees
3. **ACK** — Client confirms, connection established

---

### UDP — User Datagram Protocol

Connectionless, fast, no guarantee of delivery. Used where speed matters more than reliability (e.g. video streaming, DNS, gaming).

---

## Media Layers (3 → 2)

### Layer 3 — Network Layer (Router)

Handles transmission between hosts on **different networks**. Data at this layer is called a **Packet**.

**Functions:**
- **Routing** — finds the best path from source to destination
- **Logical Addressing** — assigns and reads IP addresses

---

### IPv4

A 32-bit address divided into 4 octets (e.g. `192.168.1.1`).

**Reserved Ranges:**

| Range | Purpose |
|---|---|
| `127.0.0.0/8` | Localhost (loopback) |
| `10.0.0.0/8` | Private LAN |
| `172.16.0.0 – 172.31.255.255` | Private LAN |
| `192.168.0.0/16` | Private LAN |

- **First IP** of any network = Network Identifier
- **Last IP** of any network = Broadcast Address

---

### Subnet Mask & CIDR

- **Subnet Mask** — defines which part of the IP is the network and which is the host
- **CIDR Notation** — shorthand for subnet mask size (e.g. `/24` instead of `255.255.255.0`)

---

### DHCP — Dynamic Host Configuration Protocol

A server that automatically assigns IP addresses to devices on a network instead of requiring manual configuration.

---

### NAT — Network Address Translation

Allows multiple devices on a private network to share a single public IP address when communicating with the internet.

---

### Layer 2 — Data Link Layer (Switch)

Responsible for **node-to-node** data transfer on the same local network. Data at this layer is called a **Frame**.

**Sublayers:**
- **LLC** (Logical Link Control)
- **MAC** (Media Access Control)

**Functions:**
- **Framing** — wraps data in frames with start/end markers
- **Physical Addressing** — adds MAC addresses to frames
- **Error Control** — detects and retransmits damaged frames
- **Flow Control** — prevents sender from overwhelming receiver
- **Access Control** — determines which device uses the channel

> **ARP (Address Resolution Protocol)** — used to discover a device's MAC address from its known IP address by broadcasting "Who has this IP?" to the network.

---

## Hardware Layer (1)

### Layer 1 — Physical Layer

Handles the actual physical transmission of raw bits over a medium (cables, radio, fiber).

**Functions:**
- **Bit Synchronization** — clock ensures sender and receiver stay in sync
- **Bit Rate Control** — defines how many bits per second are transmitted
- **Physical Topologies** — bus, star, mesh
- **Transmission Modes** — simplex, half-duplex, full-duplex

---

## Quick Reference — OSI Layers Summary

| Layer | Name | Data Unit | Key Protocol/Device |
|---|---|---|---|
| 7 | Application | Data | HTTP, DNS, FTP |
| 6 | Presentation | Data | SSL/TLS, encoding |
| 5 | Session | Data | NetBIOS, RPC |
| 4 | Transport | Segment | TCP, UDP |
| 3 | Network | Packet | IP, Router |
| 2 | Data Link | Frame | MAC, Switch |
| 1 | Physical | Bits | Cables, NIC |
