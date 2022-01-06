# Wireshark Assignment II

Mohammad Amin Ghasvari - 97521432

# Overview

## `traceroute`

This commands actually shows the possible routes and measure the transmit delays of packets across an Internet Protocol network.

```bash
🕒 23:56 ⚡ ~ 👾 traceroute github.com
traceroute to github.com (140.82.121.4), 64 hops max, 52 byte packets
 1  dsw01-rspn-fe11.respina.net (192.168.1.1)  1.434 ms  1.149 ms  1.064 ms
 2  192.168.171.42 (192.168.171.42)  38.628 ms  28.443 ms  40.604 ms
 3  192.168.171.41 (192.168.171.41)  29.949 ms  27.323 ms  50.945 ms
 ~~4  ********** ***********  33.222 ms  37.766 ms  52.613 ms~~
 5  10.201.177.14 (10.201.177.14)  36.657 ms  32.594 ms  37.624 ms
 6  10.202.4.166 (10.202.4.166)  38.255 ms
    10.21.211.10 (10.21.211.10)  24.401 ms  30.900 ms
 7  85.132.90.153 (85.132.90.153)  46.488 ms  67.054 ms
    85.132.90.165 (85.132.90.165)  40.579 ms
 8  ae305-0.ffttr7.frankfurt.opentransit.net (193.251.128.74)  107.732 ms * *
 9  * ffm-b5-link.ip.twelve99.net (62.115.155.28)  171.397 ms *
10  github-ic350972-ffm-b11.ip.twelve99-cust.net (62.115.182.171)  231.911 ms
    ffm-bb2-link.ip.twelve99.net (62.115.114.90)  111.559 ms
    ffm-bb1-link.ip.twelve99.net (62.115.114.88)  140.073 ms
11  * * *
12  * * github-ic350972-ffm-b11.ip.twelve99-cust.net (62.115.182.171)  113.504 ms
13  * * *
14  * * *
15  * * *
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
```

The size of the UDP datagram sent towards the final destination can be explicitly set by indicating the number of bytes in the datagram

```bash
traceroute gaia.cs.umass.edu 2000
```

## Capture the packets

First, I need to capture the packets. I could start capturing using Wireshark and entering the `tracroute` command, but I preferred using the author’s trace file.

To download the trace file:

```bash
🕒 0:12 ⚡ wireshark-II 👾 wget http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip
--2022-01-05 00:12:36--  http://gaia.cs.umass.edu/wireshark-labs/wireshark-traces-8.1.zip
Resolving gaia.cs.umass.edu (gaia.cs.umass.edu)... 128.119.245.12
Connecting to gaia.cs.umass.edu (gaia.cs.umass.edu)|128.119.245.12|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7191365 (6.9M) [application/zip]
Saving to: ‘wireshark-traces-8.1.zip’

wireshark-traces-8.1.zip                           100%[==============================================================================================================>]   6.86M   108KB/s    in 1m 50s

2022-01-05 00:14:27 (63.8 KB/s) - ‘wireshark-traces-8.1.zip’ saved [7191365/7191365]
```

Then unzip the zip file:

```bash
🕒 0:15 ⚡ wireshark-II 👾 unzip wireshark-traces-8.1.zip
Archive:  wireshark-traces-8.1.zip
  inflating: dns-wireshark-trace1-1.pcapng
  inflating: dns-wireshark-trace1-2.pcap
  inflating: dns-wireshark-trace2-1.pcapng
  inflating: dns-wireshark-trace2-2.pcap
  inflating: dns-wireshark-trace3-1.pcapng
  inflating: dns-wireshark-trace3-2.pcap
  inflating: http-wireshark-trace-from-george.zip
  inflating: http-wireshark-trace1-1.pcapng
  inflating: http-wireshark-trace1-2.pcapng
  inflating: http-wireshark-trace2-1.pcapng
  inflating: http-wireshark-trace2-2.pcapng
  inflating: http-wireshark-trace3-1.pcapng
  inflating: http-wireshark-trace3-2.pcapng
  inflating: http-wireshark-trace4-1.pcapng
  inflating: http-wireshark-trace4-2.pcapng
  inflating: http-wireshark-trace5-1.pcapng
  inflating: http-wireshark-trace5-2.pcapng
  inflating: intro-wireshark-trace1.pcap
  inflating: tcp-wireshark-trace1-1.pcapng
  inflating: tcp-wireshark-trace1-2.pcapng
  inflating: ip-wireshark-trace1-1.pcapng
  inflating: ip-wireshark-trace2-1.pcapng
  inflating: nat-inside-wireshark-trace1-1.pcapng
  inflating: nat-outside-wireshark-trace1-1.pcapng
  inflating: dhcp-wireshark-trace1-1.pcapng
  inflating: tls-wireshark-trace1.pcapng
  inflating: ethernet-wireshark-trace1.pcapng
```

So I load the *`ip-wireshark-trace1-1.pcapng` file to the Wireshark.*

# IPv4

❓ 1. Select the first UDP segment sent by your computer via the `traceroute` command to gaia.cs.umass.edu. (Hint: this is 44th packet in the trace file in the *`ip-wireshark-trace1-1.pcapng`* file in footnote 2). Expand the Internet Protocol part of the packet in the packet details window. What is the IP address of your computer?

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled.png)

As you can see it is `192.168.86.61`

❓ 2. What is the value in the time-to-live (TTL) field in this IPv4 datagram’s header?

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%201.png)

The TTL is `1`

❓ 3. What is the value in the upper layer protocol field in this IPv4 datagram’s header?

As you can see in the above image, It is `UDP (17)`

❓ 4. How many bytes are in the IP header?

You can see the `0101` is the header length. It’s `20`

❓ 5. How many bytes are in the payload of the IP datagram? Explain how you determined the number of payload bytes.

You need to calculate the payload size using:

$TotalLength - HeaderLength = PayloadLength$

So then it is `56-20=36`

❓ 6. Has this IP datagram been fragmented? Explain how you determined whether or not the datagram has been fragmented.

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%202.png)

The third bit is `0`, then it is not fragmented.

## UDP

❓ 7. Which fields in the IP datagram *always* change from one datagram to the next within this series of UDP segments sent by your computer destined to 128.119.245.12, via `traceroute`? Why?

- Identification field
- Checksum header is changing for data: Since the header is changing the checksum is changing too
- Time to live: Because of using `traceroute` it is changing.
- Total Length: Because the amount of data is changing

❓ 8. Which fields in this sequence of IP datagrams (containing UDP segments)﻿stay constant? Why?

- Version: since we are using IPv4 for all packets
- Destination IP: since we are sending to the same dest
- Source IP: since we are sending from the same source
- Header length: since these are UDP packets
- Differentiated Services: since all packets are UDP they use the same Type of Service class
- Upper Layer Protocol: since these are UDP packets

❓ 9. Describe the pattern you see in the values in the Identification field of the IP datagrams being sent by your computer.

I selected the first packet and moved forward. I was checking the Identification field until the 125th packet and it was increasing one by one. The next packet, the 181st, It is for the second command, Because it has a difference in time and identification field. So for each command the Identification field is ascending and increasing.

## ICMP

❓ 10. What is the upper layer protocol specified in the IP datagrams returned from the routers? [Note: the answers for Linux/MacOS differ from Windows here].

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%203.png)

As you can see in the above picture, It is `ICMP (1)`

❓ 11. Are the values in the Identification fields (across the sequence of all of ICMP packets from all of the routers) similar in behavior to your answer to question 9 above?

It doesn’t have any kind of order for different routers, But the same routers, They are ascending in identification field.

❓ 12. Are the values of the TTL fields similar, across all of ICMP packets from all of the routers? 

The answer is no. But the TTL of packets for each one of the routers are similar.

# Fragmentation

❓ 13. Has that segment been fragmented across more than one IP datagram? (Hint: the answer is yes!)

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%204.png)

These three packets are sorted based on their time and they have the same identification field. So they are fragmented.

❓ 14. What information in the IP header indicates that this datagram been fragmented?

Based on the above picture, You can look at the flag section in header. It has more fragments. So the answer is YES.

❓ 15. What information in the IP header for this packet indicates whether this is the first fragment versus a latter fragment?

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%205.png)

In the flags, from the offset field we can understand the order of these fragments. For instance, the first one, 179th is `0`, the next is `1480`, and the last one is `2960`.

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%206.png)

❓ 16. How many bytes are there in is this IP datagram (header plus payload)?

$TotalLength - HeaderLength = PayloadLength$

The total length is sum of header and payload length.

- The first packet length is 1500
- The second packet length is 1500
- The last one length is 40

Total length of these fragment is 3040.

❓ 17. Now inspect the datagram containing the second fragment of the fragmented UDP segment. What information in the IP header indicates that this is *not* the first datagram fragment?

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%207.png)

Because the offset is not 0 and the more fragments in flag is 1. So this fragment could not be the first one or the last one.

❓ 18. What fields change in the IP header between the first and second fragment?

Header Offset field and Header Checksum is changing. So the flag is different.

❓ 19. Now find the IP datagram containing the third fragment of the original UDP segment. What information in the IP header indicates that this is the last fragment of that segment?

We take a look at the headers. In flags the more fragment bit is 0, So this is the last packet of fragment. The `181st` packet is the last packet of the fragment.

# IPv6

I loaded the *`ip-wireshark-trace1-1.pcapng` into the Wireshark.*

❓ 20. What is the IPv6 address of the computer making the DNS AAAA request? This is the source address of the 20th packet in the trace. Give the IPv6 source address for this datagram in the exact same form as displayed in the Wireshark window.

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%208.png)

As you can see in the above picture, It’s `2601:193:8302:4620:215c:f5ae:8b40:a27a`

❓ 21. What is the IPv6 destination address for this datagram? Give this IPv6 address in the exact same form as displayed in the Wireshark window.

Based on the previous picture the destination IP is `2001:558:feed::1`

❓ 22. What is the value of the flow label for this datagram?

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%209.png)

The flow label for this datagram is `0x63ed0`.

❓ 23. How much payload data is carried in this datagram?

Based on the previous image, The payload length is `37`

❓ 24. What is the upper layer protocol to which this datagram’s payload will be delivered at the destination?

In the transport layer we have UDP/TCP protocol. 

Based on the previous image, the upper layer protocol is `UDP (17)`

❓ 25. How many IPv6 addresses are returned in the response to this AAAA request?

The 27th packet returned the `2607:f8b0:4006:815::200e` IP address. Just one IP address.

❓ 26. What is the first of the IPv6 addresses returned by the DNS for youtube.com (in the *`ip-wireshark-trace2-1.pcapng`* trace file, this is also the address that is numerically the smallest)? Give this IPv6 address in the exact same shorthand form as displayed in the Wireshark window.

![Untitled](Wireshark%20Assignment%20II%203574541600264ae9ae06bbee80d72e18/Untitled%2010.png)

The first returned IP address for youtube is `2607:f8b0:4006:815::200e`.