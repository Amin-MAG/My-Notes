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
