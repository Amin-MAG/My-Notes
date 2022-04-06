# Experiment 0

✏️  M.Amin Ghasvari  (97521432)

## Overview

I ran the Mininet image inside Virtual Box on the top of another windows machine. Then, I used a bridged network adapter for this virtual OS. 

## Part 1

I checked the connection using the `ping` command.

```bash
ping -c 4 192.168.1.10
```

![Untitled](Experiment%200/Untitled.png)

The RTT or Round Trip Time is in milliseconds and contains sending and acknowledging the packet. Here, RTT is `100.439ms`.

The TTL is the lifetime of data that is being sent. Here, TTL is `64s`. 

Let’s `ping` other addresses:

![Untitled](Experiment%200/Untitled%201.png)

As you can see the RTT & TTL for

- google is `250.394ms` & `120s`
- facebook is `297.246ms` & `58s`
- reddit is `227.29ms` & `59s`
- notion is `300.24ms` & `58s`

## Part 2

I turned on the Wireshark and send 5 packets to my Mininet VM.

```bash
ping -c 5 192.168.1.10
```

Here is the Result

![Untitled](Experiment%200/Untitled%202.png)

Here is the Wireshark View

![Untitled](Experiment%200/Untitled%203.png)

I filtered the `ip.dst` to be my VM IP address.

## Part 3

I used this command to jump into the VM machine.

```bash
ssh mininet@192.168.1.10
```

For the first picture, We have a single switch and 2 computers. You can see the interface connections in the `net` command. For example `s1-eth1:s2-eth4` means that the `eth1` of `s1` is connected to the `eth4` of `s2`.

```bash
# To create the network
sudo mn --topo minimal

# To test the connection
mininet> h1 ping -c 4 h2
mininet> net
```

![Untitled](Experiment%200/Untitled%204.png)

For the second picture, We need 2 switches and 4 computers.

```bash
# To create the network
sudo mn --topo linear,2,2

# To test the connection
mininet> h1s1 ping -c 4 h2s2
mininet> net
```

![Untitled](Experiment%200/Untitled%205.png)

For the last one, We need 4 switches and 9 computers.

```bash
# To create the network
sudo mn --topo linear,2,2

# To test the connection
mininet> h1 ping -c 4 h9
mininet> net
```

![Untitled](Experiment%200/Untitled%206.png)

## Part 4

Ok, so we recreate the first scenario again to fill out the table.

### Bandwidth = 100Mbps

I changed the link variations using `--link` option.

```bash
# This is the first scenario
sudo mn --topo minimal --link tc,bw=100,delay=0.01ms

# To calculate the RTT and Measured Bandwidth
h1 ping -c 4 h2
iperf
```

![Untitled](Experiment%200/Untitled%207.png)

Then I repeated the same steps for different delays to complete the table.

| Delay (ms) | RTT (ms) | Measured Bandwidth |
| --- | --- | --- |
| 0.01 | 3.680 | ['95.2 Mbits/sec', '105 Mbits/sec'] |
| 0.05 | 4.078 | ['95.4 Mbits/sec', '105 Mbits/sec'] |
| 0.1 | 3.355 | ['95.2 Mbits/sec', '105 Mbits/sec'] |
| 0.5 | 5.523 | ['95.2 Mbits/sec', '105 Mbits/sec'] |
| 1 | 7.720 | ['94.1 Mbits/sec', '104 Mbits/sec'] |
| 2 | 13.186 | ['94.6 Mbits/sec', '105 Mbits/sec'] |
| 5 | 27.939 | ['93.8 Mbits/sec', '104 Mbits/sec'] |
| 10 | 52.805 | ['92.1 Mbits/sec', '103 Mbits/sec'] |
| 50 | 253.715 | ['73.9 Mbits/sec', '83.5 Mbits/sec'] |
| 100 | 503.800 | ['45.2 Mbits/sec', '50.1 Mbits/sec'] |
| 500 | 2752.469 | ['391 Kbits/sec', '393 Kbits/sec'] |

When the delay is going to be higher, It causes traffic in our network. It’s the bottleneck, and fewer data will be sent because of this considerable delay. The RTT goes up, and We can’t use all of the link capacity to transmit data.

### Delay = 1ms

```bash
# This is the second scenario
sudo mn --topo minimal --link tc,bw=0.01,delay=1ms

# To calculate the RTT and Measured Bandwidth
h1 ping -c 4 h2
iperf
```

![Untitled](Experiment%200/Untitled%208.png)

Then I repeated the same steps for different bandwidths to complete the table.

| Bandwidth (Mbits/sec) | RTT (ms) | Measured Bandwidth |
| --- | --- | --- |
| 0.01 | 6.187 | It is so slow to measure |
| 0.05 | 6.702 | ['48.0 Kbits/sec', '187 Kbits/sec'] |
| 0.1 | 7.523 | ['96.5 Kbits/sec', '204 Kbits/sec'] |
| 0.5 | 6.452 | ['479 Kbits/sec', '871 Kbits/sec'] |
| 1 | 5.868 | ['959 Kbits/sec', '1.27 Mbits/sec'] |
| 2 | 5.911 | ['1.92 Mbits/sec', '2.26 Mbits/sec'] |
| 5 | 8.098 | ['4.78 Mbits/sec', '5.77 Mbits/sec'] |
| 10 | 7.767 | ['9.55 Mbits/sec', '11.2 Mbits/sec'] |
| 50 | 7.058 | ['47.4 Mbits/sec', '52.5 Mbits/sec'] |
| 100 | 7.426 | ['94.0 Mbits/sec', '107 Mbits/sec'] |
| 500 | 7.650 | ['427 Mbits/sec', '446 Mbits/sec'] |

With a static delay in our network, changing the bandwidth doesn’t cause considerable change in RTT. It just increases the capacity of data transmission in our network.