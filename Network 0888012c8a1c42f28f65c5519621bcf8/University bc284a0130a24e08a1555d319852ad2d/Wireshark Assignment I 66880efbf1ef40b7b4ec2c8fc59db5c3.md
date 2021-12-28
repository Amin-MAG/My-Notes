# Wireshark Assignment I

Mohammad Amin Ghasvari - 97521432

# Capturing

I had the wireshark program installed on my computer. I just downloaded the alice file from the url:

```bash
🕒 0:16 ⚡ wireshark 👾 wget http://gaia.cs.umass.edu/wireshark-labs/alice.txt
--2021-12-28 00:16:36--  http://gaia.cs.umass.edu/wireshark-labs/alice.txt
Resolving gaia.cs.umass.edu (gaia.cs.umass.edu)... 128.119.245.12
Connecting to gaia.cs.umass.edu (gaia.cs.umass.edu)|128.119.245.12|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 152138 (149K) [text/plain]
Saving to: ‘alice.txt’

alice.txt                               100%[===============================================================================>] 148.57K   127KB/s    in 1.2s

2021-12-28 00:16:38 (127 KB/s) - ‘alice.txt’ saved [152138/152138]

🕒 0:16 ⚡ wireshark 👾 ll
total 304
drwxr-xr-x    3 snapp  staff    96B Dec 28 00:16 .
drwxr-xr-x+ 120 snapp  staff   3.8K Dec 28 00:16 ..
-rw-r--r--    1 snapp  staff   149K Aug 21  2004 alice.txt
🕒 0:16 ⚡ wireshark 👾
```

So to capture the file’s packet, I needed to open the webpage and select the file that I want to upload. Then before pressing the upload key I used wireshark to listen to the outcoming packets.

So I pressed upload and then stop listening after uploading the file.

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled.png)

But at last I preferred to use the author trace file, because I have some unrelated package in `wlan` interface.

# First look

I need to filter the packets so I discover the ip address of the domain using ping command:

```bash
🕒 0:38 ⚡ Downloads 👾 ping gaia.cs.umass.edu
PING gaia.cs.umass.edu (128.119.245.12): 56 data bytes
64 bytes from 128.119.245.12: icmp_seq=0 ttl=49 time=271.290 ms
64 bytes from 128.119.245.12: icmp_seq=1 ttl=49 time=293.189 ms
64 bytes from 128.119.245.12: icmp_seq=2 ttl=49 time=312.801 ms
```

Then I filtered packets and applied this  `128.119.245.12`  for destination

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%201.png)

❓  What is the IP address and TCP port number used by the client computer (source) that is transferring the `alice.txt` file to [gaia.cs.umass.edu](http://gaia.cs.umass.edu/)?

So as we saw in this trace file the client IP Address is `192.168.86.68` and it is using the `80` port.

❓ What is the IP address of [gaia.cs.umass.edu](http://gaia.cs.umass.edu/)? On what port number is it sending and receiving TCP segments for this connection?

As we saw in the `ping` command and also in trace file the Ip Address is `128.119.245.12` and the port is `55639`.

❓ What is the sequence number of the TCP SYN segment that is used to initiate the TCP connection between the client computer and [gaia.cs.umass.edu](http://gaia.cs.umass.edu/)? What is it in this TCP segment that identifies the segment as a SYN segment?Will the TCP receiver in this session be able to use Selective Acknowledgments?

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%202.png)

When a packet is selected you can see all of the bottom, and `wireshark` makes them human readable. Here we have `Syn: Set` It means that we have SYN here, So yes we can use selective ACK. The sequence no was `seq=0`.

❓ What is the sequence number of the SYNACK segment sent by [gaia.cs.umass.edu](http://gaia.cs.umass.edu/) to the client computer in reply to the SYN? What is it in the segment that identifies the segment as a SYNACK segment? What is the value of theAcknowledgementfield in the SYNACK segment? How did [gaia.cs.umass.edu](http://gaia.cs.umass.edu/) determine that value?

The Sequence no was`seq=0`

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%203.png)

It has both ACK and SYN flags in its packet. And $ACK=Seq_{no} +1$ so the ACK is `ACK=0+1=1`

❓ What is the sequence number of the TCP segment containing the header of the HTTP POST command? How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file `alice.txt` fit into this single segment?

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%204.png)

The relative sequence number is 152041. The packet has a section at the end that showing this file is sending using `multipart` or MIME, It is obvious that the amount of transferred data is smaller than what is must be. The payload was about 1358 bytes and the `alice.txt` is 149KB.

❓ Consider the TCP segment containing the HTTP “POST”as the first segment in the data transfer part of the TCP connection.

- At what time was the firstsegment(the onecontaining the HTTP POST) in the data-transfer part of the TCP connection sent?
- At what timewas the ACK for this firstdata-containing segment received?
- What is the RTT for this first data-containing segment?
- What is the RTT value the seconddata-carrying TCP segment and its ACK?
- What is the EstimatedRTTvalue (see Section 3.5.3, in the text) after the ACK for the second data-carryingsegmentis received?

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%205.png)

So the first segment first frame arrival is 0.024047 And the ACK is 0.0526 then the difference turn out to be the RTT. `RTT=0.0526-0.024=0.0286`

The second packets’ RTT is equal to 0.028628

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%206.png)

The if we want to calculate the Estimate RTT:

Estimated RTT = $\frac{7}{8} \times 0.0286 + \frac{1}{8} \times  0.028628 = 0.0286035$

❓ What is the length (headerplus payload) ofeach ofthe first fourdata-carrying TCP segments?

So for the first four packets I just read these length and write it down.  The first one is the header and the second one is the payload.

- $32 + 777 = 809$
- $32 + 1448=1480$
- $32 + 1448=1480$
- $32 + 1448=1480$

❓ What is the minimum amount of available buffer space advertised to the client by gaia.cs.umass.eduamong these first fourdata-carrying TCP segments7?  Does the lack of receiver buffer space ever throttle the senderfor these first four data-carrying segments?

So I checked this trace file for this client and all of these packets (4 packets) has the value 131712 for the Receiver window (actually it is what the question is asking about)

![Untitled](Wireshark%20Assignment%20I%2066880efbf1ef40b7b4ec2c8fc59db5c3/Untitled%207.png)

The above packets are the first four packets, and the answer for the second part is no.

❓ Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?

For answering this question we should look for the sequence no. When we have a retransmit then there must be an example that the next packet sequence no is not bigger than the previous. But if it is always bigger than the previous one it means no retransmit, and I don’t see any retransmit in this trace file.

❓ How much data does the receiver typically acknowledgein an ACK among the first ten data-carrying segments sent from the client to gaia.cs.umass.edu? Can you identify cases where the receiver is ACKing every other received segment (see Table 3.2 in the text)among these first ten data-carrying segments?

The difference between each sequence no indicates the amount of data received there.

For Example:

```
	ACK 0 -> 566 566
	ACK 1 -> 2026 1460
```

❓ What is the throughput (bytes transferred per unit time) for the TCP connection? Explain how you calculated this value.

Throughput is $Throughput=\frac{DataSize}{time}$

Then we need to calculate the time using the first and last segment `0.192732` and also by checking first and last ack we can calculate the size of data `153426` so:

$Throughput  = \frac{153426}{0.192732} = 796053.587$

❓ Use the Time-Sequence-Graph(Stevens) plotting tool to view the sequence number versus time plot of segments being sent from the client to the [gaia.cs.umass.edu](http://gaia.cs.umass.edu/) server. Consider the “fleets”of packets sent around t= 0.025, t=0.053, t=0.082 and t=0.1. Comment on whether this looks as if TCP is in itsslow start phase, congestion avoidance phase or some other phase.Figure 6 shows a slightly different view of this data.

`t = 0.025`: Slow start phase, `t = 0.06`: Congestion Avoidance, `t = 0.08`: Congestion Avoidance, t = `0.1`: Slow start

❓ These “fleets”of segments appear to have some periodicity. What can you say about the period?

Yes as the windows gets bigger we try to avoid the congestion and It avoid window to gets bigger. (AIMD)

❓ For the last question I used the trace file!