# Network

## Models

Network Models provide a conceptual framework for understanding how different networking protocols and technologies interact within a network. The OSI model is more comprehensive, while the TCP/IP model is commonly used in practice, especially in the context of the internet.

### OSI Model

OSI Model is primarily used for educational purposes, as it provides a comprehensive and theoretical understanding of networking concepts. OSI Model has Seven layers - Physical, Data Link, Network, Transport, Session, Presentation, Application. 

- **Physical Layer**: This layer defines data transmission in electrical and physical terms.
- **Data Link Layer**: This layer defines the transmission of data between two nodes.
- **Network Layer**: It defines how packets are transmitted between networks. This layer needs to identify the hosts and networks.
- **Transport Layer**: It defines mechanisms to deliver variable length messages.
- **Session Layer**: This layer defines communication between applications running on the host.
- **Presentation Layer**: It defines common formats for data representation.
- **Application Layer**: This layer defines how user-centric applications should send and receive data.

### TCP/IP Model

The TCP/IP model is the foundation of the internet and is widely used in various networking applications. TCP/IP Model has Four layers - Network Access, Internet, Transport, Application. TCP/IP is not just a model but also a suite of protocols. Different protocols operate at each layer to facilitate communication.
#### Network Access Layer:

It deals with the physical connection between the device and the network (similar to OSI Physical Layer). It is responsible for protocols that govern data link and physical layer interactions. The Data Unit, Frame, encapsulates the packet from the Internet Layer and add physical layer information, such as MAC addresses.

- **Ethernet**
- **Wi-Fi (802.11)**
- **PPP (Point-to-Point Protocol).**

#### Internet Layer

It focuses on logical addressing and routing of packets between different networks (similar to OSI Network Layer). Internet Protocol (IP), including IPv4 and IPv6 is the core protocol of this layer. It handles fragmentation, packet forwarding, and routing. ICMP (Internet Control Message Protocol) is used for error reporting and diagnostics. Data Unit is Packet. Packets encapsulate the data from the Transport Layer and add source and destination IP addresses.

- **IP**
- **ICMP**

#### Transport Layer

It manages end-to-end communication and ensures data integrity (similar to OSI Transport Layer). Two main protocols at this layer are TCP (Transmission Control Protocol) and UDP (User Datagram Protocol). Data Unit is Segment (for TCP) or Datagram (for UDP). They encapsulate the data from the Application Layer and add source and destination port numbers.

- **TCP**: provides reliable, connection-oriented communication with features like error checking, sequencing, and flow control.
- **UDP**: provides connectionless, unreliable communication, often used for real-time applications.

#### Application Layer

It provides network services directly to end-users or applications (similar to OSI Application Layer). It encompasses a variety of protocols and services that support application-level functions. This layer is where applications interact with the network. Data Unit is Data and it is segmented into smaller units (messages, segments, or packets) depending on the specific protocol in use.

- HTTP (Hypertext Transfer Protocol)
- FTP (File Transfer Protocol)
- SMTP (Simple Mail Transfer Protocol)
- DNS (Domain Name System)
- ...

## Connection-oriented Service

### TCP Header format

![](Network/tcp-model.png)

Flags: 
- `SYN`: Triggers a synchronization of sequence numbers.
- `ACK`: Indicates that receiver should care about the acknowledgment number.
- `FIN`: Starts a process of tearing down a connection.
- `RST`: Resets the connection in case of error.

## Connection-less Service

### UDP Header Format

![](Network/udp-model.png)

## RTT or Round Trip Time

Round-trip time (RTT) is the duration, measured in milliseconds, from when a browser sends a request to when it receives a response from a server. It’s a key performance metric for web applications and one of the main factors, along with Time to First Byte (TTFB).

## Network Congestion

The network can handle the transmission of a specific amount of data. If there is more data transmission, it leads to a decrease in performance and an increase in latency. Here are some reasons:

- Heavy Network Traffic
- Network Failure
- Insufficient Bandwidth
- Malware or Cyberattacks

# See More

- [VPN](Network/VPN%2009efe.md)
- [Proxy](Proxy.md)
- [Load Balancing](Load-Balancing.md)
- [CDN](CDN.md)
- [Tor](Network/Tor%20662bd.md)
- [Setup a security camera](Network/SecurityCamera.md)
- [Laboratory](Network/Laboratory%20a293b.md)
- [University](Network/University%20bc284.md)
- [FTP](FTP.md)
- [Protocols](Protocols.md)
- [WebSocket](WebSocket.md)
- [Scapy](Scapy.md)


