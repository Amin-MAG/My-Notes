# Scapy

## Read and write pcap files

```python
d = edpcap("filename.pcab")

# Summary of layers of the frame
d.summary()
d.show()

```

The return value from read function is just like an array, To see the first packet

```python
print(d[0])

# Details about a frame
d[0].summary()
d[0].show()
```

To filter some packets you can use 

```python
filtered = [p for p in pkts if UDP in p and (p[IP].src == "17.253.20.125" or p[IP].dst  == "17.253.20.125"
```


## Sniffing

```python
pkts = sniff(iface="en0", count=100, lambda x: x.summary())
```

> **Note**: You can use `lsc` to see all of available functions.

## Create Packets and frames


```python
# Create IP packet
p = IP()

# Create two layer packet
p = IP()/TCP()

# Create multi-layer packet with raw data
p = Ether()/IP()/TCP()/"text"
```

### Ether (Ethernet Frame)

- **src:** Source MAC address.
- **dst:** Destination MAC address.
- **type:** EtherType field indicating the type of payload (e.g., IP, IPv6).

### IP (Internet Protocol)

- **src:** Sodurce IP address.
- **dst:** Destination IP address.
- **ttl:** Time-to-Live, indicating the maximum number of hops a packet can take.
- **proto:** Protocol field indicating the type of payload (e.g., TCP, UDP).

### TCP and UDP (Transport Layer)

- **sport:** Source port number.
- **dport:** Destination port number

#### TCP 

- **flag:**
	- **S (Synchronize):** Used to initiate a connection. It also establishes the sequence numbers for data exchange.
	- **P (Push):** This flag indicates that the sender has data to send and wants the receiver to process and deliver it immediately.
	- **A (Acknowledgment):** Acknowledges the successful receipt of packets. It also acknowledges the agreed-upon initial sequence numbers during the three-way handshake.
	- **F (Finish):** Indicates the end of the connection or that the sender has no more data to transmit.
	- **R (Reset):** Resets the connection. It is used to reject an invalid segment or terminate a connection abruptly.
	- **U (Urgent):** Indicates that the urgent pointer field is significant. It points to the end of the urgent data.
	- **E (ECN Echo):** Explicit Congestion Notification Echo. It is used to notify the sender of network congestion.
	- **C (Congestion Window Reduced):** Congestion Window Reduced. It indicates that the sender has reduced its congestion window.

## Send a packet

```python
p = IP()/ICMP()/"hello"
send(p)
```

## Make HTTP Request

The "HTTP Request" step involves the client sending an HTTP request to the server over the established TCP connection. See more on [HTTP page](Network/HTTP.md#HTTP%20Mechanism). To make an HTTP request, you need to follow these steps

1. Create a SYN packet 

```python
tcp_syn_packet  = scapy.IP(dst="34.23.165.48")/scapy.TCP(sport=60036, dport=8081, flags="S")
```

2. Receive the SYN-ACK packet from the server

```python
syn_ack_response = scapy.sr1(tcp_syn_packet, timeout=2)
```

> **Note**: When your kernel receives the SYN-ACK packet, It might mess up the connection. The reason is that, OS is in charge of TCP handshake. Receiving a SYN-ACK is not expected. 

If you want to block the kernel sending the RST packet, 

```bash
iptables -A OUTPUT -p tcp --tcp-flags RST RST -s 192.168.2.68 -j DROP
```

3. Send the ACK packet based on the server's response

```python
ack = (scapy.IP(dst="34.23.165.48") / scapy.TCP(seq=syn_ack_response[scapy.TCP].ack, dport=8081, sport=syn_ack_response[scapy.TCP].dport, flags="A", ack=syn_ack_response[scapy.TCP].seq+1) / f"GET /ping HTTP/1.1\r\nHost 34.23.165.48\r\n\r\n")

reply = scapy.sr1(ack, timeout=2)
```

4. Start HTTP communication using PUSH 

```python
http_request_packet = scapy.IP(dst="34.23.165.48") / scapy.TCP(dport=8081, sport=syn_ack_response[scapy.TCP].dport, flags="PA", seq=syn_ack_response[scapy.TCP].ack, ack=syn_ack_response[scapy.TCP].seq + 1) / f"GET /ping HTTP/1.1\r\nHost: 34.23.165.48\r\n\r\n"

data = scapy.sr1(http_request_packet)
```

