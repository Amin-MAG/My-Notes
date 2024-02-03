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

## Send a packet

```python
p = IP()/ICMP()/"hello"
send(p)
```