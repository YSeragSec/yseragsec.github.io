---
title: "Networking Fundamentals — Week 1: OSI Model, HTTP, DNS, TCP/UDP & More"
date: 2026-03-28 00:00:00 +0200
categories: [CATReloaded Entry Level, Networking]
tags: [networking, OSI, HTTP, DNS, TCP, UDP, IP, subnetting, DHCP, NAT, routing, fundamentals]
description: My Week 1 notes from the CAT Reloaded entry-level program. Covers the OSI model layers, DNS, HTTP, TCP/UDP, IP addressing, subnetting, DHCP, NAT, and routing.
last_modified_at: 2026-03-28 00:00:00 +0200
---


## Network

### Open System Interconnection (OSI) Model

*the OSI model is an international standard that serves as a basis for how technology vendors can describe the role of their technologies in enabling interoperability and communication and how they interact with other components.*

### Layers of OSI

- **How Data Flows in the OSI Model?**
    
    When we transfer information from one device to another, it travels through 7 layers of OSI model. First data travels down through 7 layers from the sender's end and then climbs back 7 layers on the receiver's end. Data flows through the OSI model in a step-by-step process:
    
    - **Application Layer:** Applications create the data.
    - **Presentation Layer:** Data is formatted and encrypted.
    - **Session Layer:** Connections are established and managed.
    - **Transport Layer:** Data is broken into segments for reliable delivery.
    - **Network Layer:** Segments are packaged into packets and routed.
    - **Data Link Layer:** Packets are framed and sent to the next device.
    - **Physical Layer:** Frames are converted into bits and transmitted physically.
- Image
  
    ![](/assets/img/posts/CATEntryLevel/Networking/image1.png)

- Animation
    
    ![](https://media.geeksforgeeks.org/wp-content/uploads/20241111182857579134/OSI-Model.gif)
    
- **Upper Layers**
    
    ### Layer 7 - Application Layer - DNS & HTTP
    
    ![The Application Layer serves as the ****interface between the end-user applications and the underlying network services. This layer provides protocols and services that are directly utilized by end-user applications to communicate across the network. Key functionalities of the Application Layer include resource sharing, remote file access, and network management.](image%202.png)
    
    The Application Layer serves as the ****interface between the end-user applications and the underlying network services. This layer provides protocols and services that are directly utilized by end-user applications to communicate across the network. Key functionalities of the Application Layer include resource sharing, remote file access, and network management.
    
    Examples of protocols operating at the Application Layer include [Hypertext Transfer Protocol (HTTP)](https://www.imperva.com/learn/performance/http2/) for web browsing, File Transfer Protocol (FTP) for file transfers, Simple Mail Transfer Protocol (SMTP) for email services, and Domain Name System (DNS) for resolving domain names to IP addresses. These protocols ensure that user applications can effectively communicate with each other and with servers over a network.
    
    ### **The Domain Name System (DNS)**
    
    - The Domain Name System (DNS) is a hierarchical and distributed naming system that translates human-readable domain names into IP addresses, enabling users to access websites easily.
    - **Img Working Steps of DNS**
        
        ![image.png](image%203.png)
        
        ![image.png](image%204.png)
        
        ![image.png](image%205.png)
        
        ![image.png](image%206.png)
        
        ![image.png](image%207.png)
        
    - **Working of DNS**
        - **User Input:** You enter a website address (for example, www.geeksforgeeks.org) into your web browser.
        - **Local Cache Check:** Your browser first checks its local cache to see if it has recently looked up the domain. If it finds the corresponding IP address, it uses that directly without querying external servers.
        - **DNS Resolver Query:** If the IP address isn’t in the local cache, your computer sends a request to a DNS resolver. The resolver is typically provided by your Internet Service Provider (ISP) or your network settings.
        - **Root DNS Server:** The resolver sends the request to a root [**DNS server**](https://www.geeksforgeeks.org/computer-networks/dns-server/). The root server doesn’t know the exact IP address for www.geeksforgeeks.org but knows which Top-Level Domain (TLD) server to query based on the domain’s extension (e.g., .org).
        - **TLD Server:** The TLD server for .org directs the resolver to the authoritative DNS server for geeksforgeeks.org.
        - **Authoritative DNS Server:** This server holds the actual DNS records for geeksforgeeks.org, including the IP address of the website’s server. It sends this IP address back to the resolver.
        - **Final Response:** The DNS resolver sends the IP address to your computer, allowing it to connect to the website’s server and load the pa**ge.**
    
    ![image.png](image%208.png)
    
    ![image.png](image%209.png)
    
    ### **Hypertext Transfer Protocol (**HTTP)
    
    - **IMG**
        
        ![image.png](image%2010.png)
        
        ![image.png](image%2011.png)
        
    - **HTTP Requests**
        
        HTTP Requests are the message sent by the client to request data from the server or to perform some actions. Different HTTP requests are:
        
        - **GET:** GET request is used to read/retrieve data from a web server. GET returns an HTTP status code of **200 (OK)** if the data is successfully retrieved from the server.
        - **POST:** POST request is used to send data (file, form data, etc.) to the server. On successful creation, it returns an HTTP status code of **201**.
        - **PUT:** A PUT request is used to modify the data on the server. It replaces the entire content at a particular location with data that is passed in the body payload. If there are no resources that match the request, it will generate one.
        - **PATCH:** PATCH is similar to PUT request, but the only difference is, it modifies a part of the data. It will only replace the content that you want to update.
        - **DELETE:** A ****DELETE request is used to delete the data on the server at a specified location.
    - **Headers**
        - **Common Request Headers**
            
            These are headers that are sent from the client (usually your browser) to the server.
            
            **Host:** Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll just receive the default website for the server.
            
            **User-Agent:** This is your browser software and version number, telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript and CSS are only available in certain browsers.
            
            **Content-Length:** When sending data to a web server such as in a form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any data.
            
            **Accept-Encoding:** Tells the web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.
            
            **Cookie:** Data sent to the server to help remember your information (see cookies task for more information).
            
            - HTTP headers contain text information stored in key-value pairs, and they are included in every HTTP request (and response, more on that later). These headers communicate core information, such as what browser the client is using and what data is being requested.
                
                Example of HTTP request headers from Google Chrome's network tab:
                
                ![HTTP request headers](https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-request-headers.png)
                
        - **Common Response Headers**
            
            These are the headers that are returned to the client from the server after a request.
            
            **Set-Cookie:** Information to store which gets sent back to the web server on each request (see cookies task for more information).
            
            **Cache-Control:** How long to store the content of the response in the browser's cache before it requests it again.
            
            **Content-Type:** This tells the client what type of data is being returned, i.e., HTML, CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header the browser then knows how to process the data.
            
            **Content-Encoding:** What method has been used to compress the data to make it smaller when sending it over the internet.
            
            - Much like an HTTP request, an HTTP response comes with headers that convey important information such as the language and format of the data being sent in the response body.
                
                Example of HTTP response headers from Google Chrome's network tab:
                
                ![HTTP response headers](https://www.cloudflare.com/img/learning/ddos/glossary/hypertext-transfer-protocol-http/http-response-headers.png)
                
    - **HTTP Status Codes**
        
        **HTTP Status Codes:**
        
        | **100-199 - Information Response** | These are sent to tell the client the first part of their request has been accepted and they should continue sending the rest of their request. These codes are no longer very common. |
        | --- | --- |
        | **200-299 - Success** | This range of status codes is used to tell the client their request was successful. |
        | **300-399 - Redirection** | These are used to redirect the client's request to another resource. This can be either to a different webpage or a different website altogether. |
        | **400-499 - Client Errors** | Used to inform the client that there was an error with their request. |
        | **500-599 - Server Errors** | This is reserved for errors happening on the server-side and usually indicate quite a major problem with the server handling the request. |
        
        **Common HTTP Status Codes:**
        
        | **200 - OK** | The request was completed successfully. |
        | --- | --- |
        | **201 - Created** | A resource has been created (for example a new user or new blog post). |
        | **301 - Moved Permanently** | This redirects the client's browser to a new webpage or tells search engines that the page has moved somewhere else and to look there instead. |
        | **302 - Found** | Similar to the above permanent redirect, but as the name suggests, this is only a temporary change and it may change again in the near future. |
        | **400 - Bad Request** | This tells the browser that something was either wrong or missing in their request. This could sometimes be used if the web server resource that is being requested expected a certain parameter that the client didn't send. |
        | **401 - Not Authorised** | You are not currently allowed to view this resource until you have authorised with the web application, most commonly with a username and password. |
        | **403 - Forbidden** | You do not have permission to view this resource whether you are logged in or not. |
        | **405 - Method Not Allowed** | The resource does not allow this method request, for example, you send a GET request to the resource /create-account when it was expecting a POST request instead. |
        | **404 - Page Not Found** | The page/resource you requested does not exist. |
        | **500 - Internal Service Error** | The server has encountered some kind of error with your request that it doesn't know how to handle properly. |
        | **503 - Service Unavailable** | This server cannot handle your request as it's either overloaded or down for maintenance. |
    
    ### Layer 6 - Presentation Layer
    
    ![The Presentation Layer, also known as the syntax layer, is responsible for translating data between the application layer and the network format. It ensures that data sent from the application layer of one system is readable by the application layer of another system. This layer handles data formatting, [encryption](https://www.imperva.com/learn/data-security/data-encryption/), and compression, facilitating interoperability between different systems.](image%2012.png)
    
    The Presentation Layer, also known as the syntax layer, is responsible for translating data between the application layer and the network format. It ensures that data sent from the application layer of one system is readable by the application layer of another system. This layer handles data formatting, [encryption](https://www.imperva.com/learn/data-security/data-encryption/), and compression, facilitating interoperability between different systems.
    
    ### **Functions of the Presentation Layer**
    
    - **Translation:** For example, [**ASCII to EBCDIC**](https://www.geeksforgeeks.org/computer-organization-architecture/difference-between-ascii-and-ebcdic/).
    - **Encryption/ Decryption:** Data encryption translates the data into another form or code. The encrypted data is known as the ciphertext, and the decrypted data is known as plain text. A key value is used for encrypting as well as decrypting data.
    - **Compression:** Reduces the number of bits that need to be transmitted on the network.
    
    ### Layer 5 - Session Layer
    
    ![[**Session Layer**](https://www.geeksforgeeks.org/computer-networks/session-layer-in-osi-model/) in the OSI Model is responsible for the establishment of connections, management of connections, terminations of sessions between two devices. It also provides authentication and security.](image%2013.png)
    
    [**Session Layer**](https://www.geeksforgeeks.org/computer-networks/session-layer-in-osi-model/) in the OSI Model is responsible for the establishment of connections, management of connections, terminations of sessions between two devices. It also provides authentication and security.
    
    ### **Functions of the Session Layer**
    
    - **Session Establishment, Maintenance, and Termination:** The layer allows the two processes to establish, use, and terminate a connection.
    - **Synchronization:** This layer allows a process to add checkpoints that are considered synchronization points in the data. These synchronization points help to identify the error so that the data is re-synchronized properly, and ends of the messages are not cut prematurely, and data loss is avoided.
    - **Dialog Controller:** The session layer allows two systems to start communication with each other in half-duplex or full duplex.
    
    **Example**
    
    Let us consider a scenario where a user wants to send a message through some Messenger application running in their browser. The “Messenger” here acts as the application layer which provides the user with an interface to create the data. This message or so-called Data is compressed, optionally encrypted (if the data is sensitive), and converted into bits (0’s and 1’s) so that it can be transmitted.
    
- **Heart of OSI - End-to-end delivery & error control**
    
    ### Layer 4 - Transport Layer - TCP & UDP
    
    ![The [**Transport Layer**](https://www.geeksforgeeks.org/computer-networks/transport-layer-in-osi-model/) provides services to the application layer and takes services from the network layer. The data in the transport layer is referred to as Segments. It is responsible for the end-to-end delivery of the complete message.](image%2014.png)
    
    The [**Transport Layer**](https://www.geeksforgeeks.org/computer-networks/transport-layer-in-osi-model/) provides services to the application layer and takes services from the network layer. The data in the transport layer is referred to as Segments. It is responsible for the end-to-end delivery of the complete message.
    
    - The transport layer also provides the acknowledgment of the successful data transmission and re-transmits the data if an error is found.
    - Protocols used in Transport Layer are [**TCP**](https://www.geeksforgeeks.org/computer-networks/what-is-transmission-control-protocol-tcp/), [**UDP**](https://www.geeksforgeeks.org/computer-networks/user-datagram-protocol-udp/) [**NetBIOS**](https://www.geeksforgeeks.org/ethical-hacking/what-is-netbios-enumeration/), [**PPTP**](https://www.geeksforgeeks.org/computer-networks/pptp-full-form/).
    - At the sender's side, the transport layer receives the formatted data from the upper layers, performs Segmentation, and also implements Flow and error control to ensure proper data transmission.
    - It also adds Source and Destination [**port number**](https://www.geeksforgeeks.org/computer-networks/what-is-ports-in-networking/) in its header and forwards the segmented data to the Network Layer.
    - Generally, this destination port number is configured, either by default or manually.
    - **Example:** when a web application requests a web server, it typically uses port number 80, because this is the default port assigned to web applications. Many applications have default ports assigned.
    - At the Receiver’s side, Transport Layer reads the port number from its header and forwards the Data which it has received to the respective application. It also performs sequencing and reassembling of the segmented data.
    
    ### **Functions of the Transport Layer**
    
    - **Segmentation and Reassembly:** This layer accepts the message from the (session) layer and breaks the message into smaller units. Each of the segments produced has a header associated with it. The transport layer at the destination station reassembles the message.
    - **Service Point Addressing:** To deliver the message to the correct process, the transport layer header includes a type of address called service point address or port address. Thus, by specifying this address, the transport layer makes sure that the message is delivered to the correct process.[https://www.geeksforgeeks.org/computer-networks/what-is-transmission-control-protocol-tcp/](https://www.geeksforgeeks.org/computer-networks/what-is-transmission-control-protocol-tcp/)
    - **Ports**
        
        ![image.png](image%2015.png)
        
    - **Transmission Control Protocol (TCP)**
        
        ![image.png](image%2016.png)
        
        - **Connection Establishment (Three-Way Handshake)**
        
        ![1. **SYN (Synchronize):** The sender sends a SYN segment to the receiver to request a connection.
        2. **SYN-ACK (Synchronize-Acknowledge):** The receiver responds with a SYN-ACK segment, acknowledging the request and agreeing to the connection.
        3. **ACK (Acknowledge):** The sender replies with an ACK, confirming the connection is established.](image%2017.png)
        
        1. **SYN (Synchronize):** The sender sends a SYN segment to the receiver to request a connection.
        2. **SYN-ACK (Synchronize-Acknowledge):** The receiver responds with a SYN-ACK segment, acknowledging the request and agreeing to the connection.
        3. **ACK (Acknowledge):** The sender replies with an ACK, confirming the connection is established.
        
    - **User Datagram Protocol - UDP**
        
        ![image.png](image%2018.png)
        
        ![image.png](75ac922f-fb1d-423f-809e-22c3cf0319b3.png)
        
    
- **Media Layer**
    
    ### Layer 3 - Network Layer - Router
    
    ![The [**Network Layer**](https://www.geeksforgeeks.org/computer-networks/network-layer-in-osi-model/) works for the transmission of data from one host to the other located in different networks. It also takes care of packet routing i.e. selection of the shortest path to transmit the packet, from the number of routes available.](image%2019.png)
    
    The [**Network Layer**](https://www.geeksforgeeks.org/computer-networks/network-layer-in-osi-model/) works for the transmission of data from one host to the other located in different networks. It also takes care of packet routing i.e. selection of the shortest path to transmit the packet, from the number of routes available.
    
    - The sender and receiver's **IP [address](https://www.geeksforgeeks.org/computer-science-fundamentals/what-is-an-ip-address/)** are placed in the header by the network layer. Segment in the Network layer is referred to as Packet**.**
    - Network layer is implemented by networking devices such as [**routers and switches**](https://www.geeksforgeeks.org/computer-networks/difference-between-router-and-switch/).
    
    ### **Functions of the Network Layer**
    
    - **Routing:** The network layer protocols determine which route is suitable from source to destination. This function of the network layer is known as routing.
    - **Logical Addressing:** To identify each device inter-network uniquely, the network layer defines an addressing scheme. The sender and receiver’s IP addresses are placed in the header by the network layer. Such an address distinguishes each device uniquely and universally.
    
    ### IPv4
    
    ![image.png](image%2020.png)
    
    ### Reserved Addresses
    
    - 127.0.0.0 - 127.255.255.255
        - Reserved for localhost.
    - 10.0.0.0 - 10.255.255.255
        - Reserved for local network (LAN).
    - 172.16.0.0 - 172.31.255.255
        - Reserved for local network (LAN).
    - 192.168.0.0 - 192.168.255.255
        - Reserved for local network (LAN).
    - First IP of any network is reserved for the
    network itself (Network Identifier).
    - Last IP of any network is reserved for the
    broadcast (Broadcast Address).
    
    ![image.png](image%2021.png)
    
    ### Subnet Mask &  DHCP
    
    ![بيحدد الجزء الخاص بالشبكة والجزء الخاص بالجهاز داخل عنوان الـIP.
    ](74761642-5b6e-4f78-80d6-5c0d2e5588d2.png)
    
    بيحدد الجزء الخاص بالشبكة والجزء الخاص بالجهاز داخل عنوان الـIP.
    
    ![طريقة مختصرة لكتابة حجم الشبكة بعدد البِتات (/24 مثلاً) بدل أرقام الـSubnet Mask.
    ](image%2022.png)
    
    طريقة مختصرة لكتابة حجم الشبكة بعدد البِتات (/24 مثلاً) بدل أرقام الـSubnet Mask.
    
    ![image.png](image%2023.png)
    
    ![سيرفر بيوزع عناوين IP تلقائيًا على الأجهزة بدل ما تكتبها يدويًا.](image%2024.png)
    
    سيرفر بيوزع عناوين IP تلقائيًا على الأجهزة بدل ما تكتبها يدويًا.
    
    ![image.png](image%2025.png)
    
    ### Routing & NAT
    
    ![image.png](094ad893-0e2d-42e1-a0ab-f7543a1ae4c6.png)
    
    ![image.png](image%2026.png)
    
    ![image.png](image%2027.png)
    
    ![image.png](image%2028.png)
    
    ![image.png](image%2029.png)
    
    ![image.png](image%2030.png)
    
    ![image.png](image%2031.png)
    
    ![image.png](image%2032.png)
    
    ![image.png](image%2033.png)
    
    ### Layer 2 - Data Link Layer - Switch
    
    ![The Data Link Layer is responsible for node-to-node data transfer and error detection and correction. It ensures that data is transmitted to the correct device on a local network segment. This layer manages [MAC (Media Access Control)](https://www.imperva.com/learn/application-security/broken-object-level-authorization-bola/) addresses and is divided into two sublayers: Logical Link Control (LLC) and Media Access Control (MAC).](image%2034.png)
    
    The Data Link Layer is responsible for node-to-node data transfer and error detection and correction. It ensures that data is transmitted to the correct device on a local network segment. This layer manages [MAC (Media Access Control)](https://www.imperva.com/learn/application-security/broken-object-level-authorization-bola/) addresses and is divided into two sublayers: Logical Link Control (LLC) and Media Access Control (MAC).
    
    - When a packet arrives in a network, it is the responsibility of the DLL to transmit it to the Host using its [**MAC address**](https://www.geeksforgeeks.org/computer-networks/mac-address-in-computer-network/).
    - Packet in the Data Link layer is referred to as Frame. [**Switches and Bridges**](https://www.geeksforgeeks.org/computer-networks/difference-between-switch-and-bridge/) are common Data Link Layer devices.
    - The packet received from the Network layer is further divided into frames depending on the frame size of the NIC ([**Network Interface Card)**](https://www.geeksforgeeks.org/computer-networks/nic-full-form/). DLL also encapsulates Sender and Receiver’s MAC address in the header.
    - The Receiver’s MAC address is obtained by placing an **ARP ([Address Resolution](https://www.geeksforgeeks.org/ethical-hacking/how-address-resolution-protocol-arp-works/) Protocol)** request onto the wire asking, "Who has that IP address?" and the destination host will reply with its MAC address.
    
    ### **Sublayers of Data Link Layer**
    
    - [**Logical Link Control (LLC)**](https://www.geeksforgeeks.org/computer-networks/logical-link-control-llc-protocol-data-unit/)
    - [**Media Access Control (MAC)**](https://www.geeksforgeeks.org/computer-networks/mac-address-in-computer-network/)
    
    ### **Functions of the Data Link Layer**
    
    - **Framing:** Framing is a function of the data link layer. It provides a way for a sender to transmit a set of bits that are meaningful to the receiver. This can be accomplished by attaching special bit patterns to the beginning and end of the frame.
    - **Physical Addressing:** After creating frames, the Data link layer adds physical addresses (MAC addresses) of the sender and/or receiver in the header of each frame.
    - **Error Control:** The data link layer provides the mechanism of error control in which it detects and retransmits damaged or lost frames.
    - **Flow Control:** The data rate must be constant on both sides else the data may get corrupted thus, flow control coordinates the amount of data that can be sent before receiving an acknowledgment.
    - **Access Control:** When a single communication channel is shared by multiple devices, the MAC sub-layer of the data link layer helps to determine which device has control over the channel at a given time.
    
    ![image.png](image%2035.png)
    
- **Hardware Layer**
    
    ### Layer 1 - Physical Layer - Raw
    
    ![The Physical Layer is responsible for the physical connection between devices. It defines the hardware elements involved in the network, including cables, switches, and other physical components. This layer also specifies the electrical, optical, and radio characteristics of the network.](image%2036.png)
    
    The Physical Layer is responsible for the physical connection between devices. It defines the hardware elements involved in the network, including cables, switches, and other physical components. This layer also specifies the electrical, optical, and radio characteristics of the network.
    
    ### **Functions of the Physical Layer**
    
    - **Bit Synchronization:** The physical layer provides the synchronization of the bits by providing a clock. This clock controls both sender and receiver thus providing synchronization at the bit level.
    - **Bit Rate Control:** The Physical layer also defines the transmission rate i.e. the number of bits sent per second.
    - **Physical Topologies:** Physical layer specifies how the different, devices/nodes are arranged in a network i.e. [**bus topology**](https://www.geeksforgeeks.org/computer-networks/advantages-and-disadvantages-of-bus-topology/), [**star topology**](https://www.geeksforgeeks.org/computer-networks/advantages-and-disadvantages-of-star-topology/), or [**mesh topology**](https://www.geeksforgeeks.org/computer-networks/advantage-and-disadvantage-of-mesh-topology/).
    - **Transmission Mode:** Physical layer also defines how the data flows between the two connected devices. The various transmission modes possible are [**Simplex, half-duplex and**](https://www.geeksforgeeks.org/computer-networks/difference-between-simplex-half-duplex-and-full-duplex-transmission-modes/) full duplex.
    
