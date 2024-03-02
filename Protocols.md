# Protocols

- [**TCP/IP (Transmission Control Protocol/Internet Protocol)**](Network/TCP-IP.md)
- [**HTTP/HTTPS (Hypertext Transfer Protocol/Secure)**](Network/HTTP.md)
- [**DNS (Domain Name System)**](Network/DNS.md)

1. **UDP (User Datagram Protocol):**
    - A connectionless protocol often used for real-time applications where low latency is crucial.
2. **BGP (Border Gateway Protocol):**
    - A routing protocol used to exchange routing and reachability information between autonomous systems on the Internet.
3. **OSPF (Open Shortest Path First):**
    - A routing protocol commonly used within an enterprise network.
4. **VXLAN (Virtual Extensible LAN):**
    - A network virtualization technology that enables the creation of logical networks for virtual machines.
5. **MPLS (Multiprotocol Label Switching):**
    - A protocol for speeding up and shaping network traffic flows.
6. **SNMP (Simple Network Management Protocol):**
    - Facilitates the exchange of management information between network devices.
7. **SMTP (Simple Mail Transfer Protocol):**
    - Used for the transmission of email.
8. **SSH (Secure Shell):**
    - Provides secure access to a remote device over a network.
9. **TLS/SSL (Transport Layer Security/Secure Sockets Layer):**
    - Protocols that provide secure communication over a computer network.
10. **DHCP (Dynamic Host Configuration Protocol):**
    - Automates the assignment of IP addresses and other network configuration information to devices.
11. **ICMP (Internet Control Message Protocol):**
    - Used for error reporting and diagnostics in IP networks.
12. **ARP (Address Resolution Protocol):**
    - Maps an IP address to a physical MAC address.
13. **IGMP (Internet Group Management Protocol):**
    - Used by hosts and adjacent routers on an Internet Protocol (IP) network to establish multicast group memberships.
14. **LDAP (Lightweight Directory Access Protocol):**
    - A protocol for accessing and maintaining distributed directory information services.
15. **RTP (Real-time Transport Protocol):**
    - Used for delivering audio and video over IP networks.
16. **SIP (Session Initiation Protocol):**
    - A signaling protocol used for initiating, maintaining, modifying, and terminating real-time sessions that involve video, voice, messaging, and other communications applications.
17. **NTP (Network Time Protocol):**
    - Synchronizes the clocks of computer systems over a network.

## SNMP

This protocol is designed for troubleshooting the network. Assume you need to trace to find which components of your network is not working properly. It can be time-consuming in a large network system. 

SNMP protocol which uses UDP/161 and UDP/162 can report the status of network components to the Management Server.

UDP/161 is a polling mechanism to retrieve data from components. The management server ask questions about the status of the components and they also respond to this question.

On the other hand, UDP/162 is called trap SNMP. This mechanism does not rely on management server's questions. Whenever something bad is going to happen, the component will send a message to the management server.

> **Note**: We do not have to choose between Polling and Traps. It is possible to use mixture of them for monitoring the status of the network.

The management server will 

- Record the data for historical use
- Present health information as graphs
- Provide a web base dashboard
- Send alerts
