# Experiment 1

✏️  M.Amin Ghasvari  (97521432)

## First of all

I ran the `Mininet` image inside Virtual Box on the top of another windows machine. Then, I used a bridged network adapter for this virtual OS. 

In some cases, I used this command to jump into the VM machine from another machine in my local network.

```bash
ssh mininet@192.168.1.10
```

## Part 1

I just added the hosts and switches in the code. Then I added the links between the hosts and switches and specified which interfaces should be connected.

```python
#!/usr/bin/python

from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI
from mininet.log import setLogLevel, info

def experiment_network():
    net = Mininet()
    info('*** Adding controller\n')
    net.addController('c0')

    info('*** Adding hosts\n')
    h1 = net.addHost('h1')
    h2 = net.addHost('h2')
    h3 = net.addHost('h3')
    h4 = net.addHost('h4')

    info('*** Adding switch\n')
    s14 = net.addSwitch('s14')
    s24 = net.addSwitch('s24')
    s34 = net.addSwitch('s34')

    info('*** Creating links\n')
    net.addLink(h1, s14, intfName1="h1-eth0", intfName2="s14-eth1")
    net.addLink(h2, s24, intfName1="h2-eth0", intfName2="s24-eth1")
    net.addLink(h3, s34, intfName1="h3-eth0", intfName2="s34-eth1")
    net.addLink(h4, s14, intfName1="h4-eth0", intfName2="s14-eth2")
    net.addLink(h4, s24, intfName1="h4-eth1", intfName2="s24-eth2")
    net.addLink(h4, s34, intfName1="h4-eth2", intfName2="s34-eth2")

    info('*** Starting network\n')
    net.start()

    # This is used to run commands on the hosts
    info('*** Starting terminals on hosts\n')
    h1.cmd('xterm -xrm "XTerm.vt100.allowTitleOps: false" -T h1 &')
    h2.cmd('xterm -xrm "XTerm.vt100.allowTitleOps: false" -T h2 &')
    h3.cmd('xterm -xrm "XTerm.vt100.allowTitleOps: false" -T h3 &')
    h4.cmd('xterm -xrm "XTerm.vt100.allowTitleOps: false" -T h4 &')

    info('*** Running the command line interface\n')
    CLI(net)
    info('*** Closing the terminals on the hosts\n')
    h1.cmd("killall xterm")
    h2.cmd("killall xterm")
    h3.cmd("killall xterm")
    h4.cmd("killall xterm")

    info('*** Stopping network')
    net.stop()

if __name__ == '__main__':
    setLogLevel('info')
    experiment_network()
```

To execute the code on the `Mininet` VM:

```bash
sudo python lanTopology.py
```

Here is the result:

![Untitled](Experiment%20fe745/Untitled.jpeg)

Instantly it opened four shell windows named h1, h2, h3, and h4.

![Untitled](Experiment%20fe745/Untitled%201.jpeg)

## Part 2

I used this command to check the hosts’ interfaces.

```bash
ip link
```

This is the result on host1 and host4:

![Untitled](Experiment%20fe745/Untitled%202.jpeg)

All interfaces are up, but we can use this command to change the state of interfaces.

```bash
ip link set <INTERFACE> up
```

I opened Wireshark on host1 using the command `wireshark`. (`wireshark &` to run in the background)

I used `ifconfig` in the host4 to see the `h4-eth0` IP address. (It was `10.0.0.4`)

I selected the `h1-eth0` as the interface and started listening to the interface and ping host4 from host1.

❓ Question 1

At first, an ARP packet was sent to ask `who has 10.0.0.4` and after that ICMP packets were sent to the destination. The first packet finds the MAC address of the destination.

I used `arp -a` to see the arp table in host1.

Now you can see in the picture that we have the MAC address of `10.0.0.4`.

![Untitled](Experiment%20fe745/Untitled%203.jpeg)

Yes, It had an ARP reply packet too, and it was asking about `who has 10.0.0.1`. After that last ARP packet, host4 also had the MAC address of host1 in its arp table.

I checked the MAC addresses, and they were ok. 

## Part 3

I used this command to change the IP addresses for interfaces as mentioned in the question.

```bash
ip addr add 10.10.14.1/24 dev h1-eth0
```

Just like the picture below

![Untitled](Experiment%20fe745/Untitled%204.jpeg)

> I used `sudo ip addr del` to fix the issue here.
> 

The result looks ok.

![Untitled](Experiment%20fe745/Untitled%205.jpeg)

Then, again I opened Wireshark on host1 and tried to ping host4.

❓ Question 2

Here again, first, it asked about `who is 10.10.14.1` with ARP packets, and after that, ICMP packets were sent to the destination. 

Yes, It had an ARP reply packet too. I saw added IP address and MAC address in both hosts’ arp table using `arp -a`.

## Part 4

❓ Question 3

I used the `ping` command to check the connection, but of course, they can not `ping` each other.

Due to subnet, they have different networks, and they are not reachable!

❓ Question 4

![Untitled](Experiment%20fe745/Untitled.png)

As you can see, it will send and receive all packets that want to communicate to the `10.10.14.0/24` range with `h1-eth0`.

❓ Question 5

So as it was mentioned,

```bash
ip route add default via 10.10.14.4
ping 10.10.34.4
```

Yes, The ICMP packets were sent to the interface. Because we told the host1 that the default gateway is host4 so that host4 can see its interface.

❓ Question 6

Still, we are not able to ping host3 from host1.

![Untitled](Experiment%20fe745/Untitled%206.jpeg)

So I enabled IP forwarding on host4.

And again tried to ping host3.

![Untitled](Experiment%20fe745/Untitled%207.jpeg)

As you can see, packets were sent to the host3, but still, I could not have the responses.

Here was why host1 could send packets to the host3, but host3 could not respond to these packets, because we didn’t add the default gateway for host3.

```bash
# In host3
ip route add default via 10.10.34.4
```

Now everything seems ok!  🎉

❓ Question 7

```bash
# In host2
ip route add default via 10.10.24.4
```

I could access

- host2 from host1
- host3 from host2
- host1 from host3

![Untitled](Experiment%20fe745/Untitled%208.jpeg)

The average RTT is very close to each other because they traverse the same way.

Source host → Switch → Host4 → Switch → Destination Host

And then, the reverse of the above path to comes back the source host.
