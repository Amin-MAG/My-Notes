# Experiment 3

## Part 1 - Get started

Like the previous experiment, I create the hosts and switches with the python code.

In some cases, I used this command to jump into the VM machine from another machine in my local network.

```bash
ssh mininet@192.168.1.10
```

## Hub

It's a component that repeats incoming packets and sends them to its other ports.

![Untitled](Experiment%203/Untitled.png)

Here is the code to implement this network. (It has some invalid data, though)

```python
#!/usr/bin/python
""" This example shows how to create a Mininet object and add nodes to
it manually.  """

"Importing Libraries"
from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI
from mininet.log import setLogLevel, info

"Function definition:  This is called from the main function"
def firstNetwork():

	"Create an empty network and add nodes to it."
	net = Mininet()

	info( '*** Adding controller\n' ) 
	net.addController( 'c0' )

	info( '*** Adding hosts \n' )
	h1 = net.addHost( 'h1', ip='10.0.0.1/24' ) 
	h2 = net.addHost( 'h2', ip='10.0.0.2/31' )
	h3 = net.addHost( 'h3', ip='10.0.0.3/24' ) 
	h4 = net.addHost( 'h4', ip='10.0.1.4/24' ) 

	info( '*** Adding switches\n' ) 
	s12 = net.addSwitch( 's12' )
	s34 = net.addSwitch( 's34' )

	info( '*** Creating links\n' ) 
	net.addLink( h1, s12 ) 
	net.addLink( h2, s12 )
	net.addLink( h3, s34 )
	net.addLink( h4, s34 )
	net.addLink( s12, s34 )

	info( '*** Starting network\n') 
	net.start()
	"This is used to run commands on the hosts"

	info( '*** Starting xterm on hosts\n' )
	h1.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h1 &') 
	h2.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h2 &')
	h3.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h3 &')
	h4.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h4 &')

	info( '*** Running the command line interface\n' ) 
	CLI( net )

	info( '*** Closing the terminals on the hosts\n' ) 
	h1.cmd("killall xterm")
	h2.cmd("killall xterm")
	h3.cmd("killall xterm")
	h4.cmd("killall xterm")

	info( '*** Stopping network' )
	net.stop()

"main Function: This is called when the Python file is run" 
if __name__ == '__main__':
	setLogLevel( 'info' )
	firstNetwork()
```

We need to change the switches to hubs, so we need to run this command in the mininet console.

```bash
mininet> sh ovs-ofctl add-flow s12 action=flood
mininet> sh ovs-ofctl add-flow s34 action=flood
```

❓ Question 1

There is an issue with the subnet mask of host 2. I need to change this to what the scenario desires.

```bash
ifconfig h2-eth0 subnet 255.255.255.0
```

Host 4 is not connected to the network because of its IP address. We can change this to `10.0.0.4` to fix the issue.

```bash
ifconfig h4-eth0 10.0.0.4 subnet 255.255.255.0
```

❓ Question 2

I started the Wireshark on all of these hosts. Then I sent a single ICMP packet from host 1 to host 2.

![Untitled](Experiment%203/Untitled%201.png)

The packet was sent successfully. Here is the difference, Wireshark captured the ARP & ICMP packets in each host (The packets are similar). The reason was that the hubs were repeating the packet that host 1 sent and the packet that host 2 received.

## Switch

A network switch forwards data packets between devices. The process of forwarding packets is based on the MAC address.

To convert the `s12` to switch, I executed this command.

```bash
mininet> sh ovs-ofctl add-flow s12 action=normal
```

To clean all records in the ARP table, I executed this command.

```bash
ip -s -s neigh flush all
```

I started the Wireshark on all of these hosts. Then I sent a single ICMP packet from host 1 to host 2.

![Untitled](Experiment%203/Untitled%202.png)

❓ Question 3

- Host 3 and 4 received just a single ARP packet.
- Host 1 and 2 received ARP and ICMP packets.

❓ Question 4

As you can see, host 1 only sent the ICMP packets to host 2. There is no captured packet except the first ARP packet in the other hosts. The packets are not repeated like the scenario that has hubs, and the forwarding is smarter.

❓ Question 5

Then I sent a single ICMP packet from host 1 to host 4.

Network in unreachable!

> If we had changed the IP address of host 4, the results would be the same as the next step. I mean, there would be some packets that were sent for both hosts 3 and 4.
> 

❓ Question 6

Then I sent a single ICMP packet from host 1 to host 3.

![Untitled](Experiment%203/Untitled%203.png)

Then, I saw the ARP and ICMP packets captured in hosts 1 and 3. Host 2 just received a single ARP packet (`who is 10.0.0.3`). The interesting point is that host 4 received the ARP and ICMP packets too because the host 3 connection to the network is via the hub, and the packets are repeated for host 4.

❓ Question 7

As the results showed, The MAC address is something that a switch uses to forward the packets. The incoming packets from host 3 have a unique MAC address like the host four packets do.

The incoming packets MAC address for host 1 is equal to the sending packets MAC address from host 3 and vice versa.

## Router

I exited from the mininet program and used this command to clean up before running the next scenario.

![Untitled](Experiment%203/Untitled%204.png)

```bash
sudo mininet -c

# Running following scenario
python3 topo2.py
```

The python code to create this scenario

```python
#!/usr/bin/python
""" This example shows how to create a Mininet object and add nodes to
it manually.  """

"Importing Libraries"
from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI
from mininet.log import setLogLevel, info

"Function definition:  This is called from the main function"
def firstNetwork():

	"Create an empty network and add nodes to it."
	net = Mininet()

	info( '*** Adding controller\n' ) 
	net.addController( 'c0' )

	info( '*** Adding hosts \n' )
	h1 = net.addHost( 'h1', ip='10.0.0.1/24') 
	h2 = net.addHost( 'h2', ip='10.0.0.2/24')
	h3 = net.addHost( 'h3', ip='10.0.1.3/24') 

	info( '*** Adding router \n' )
	r1 = net.addHost( 'r1', ip='10.0.0.100/24' ) 

	info( '*** Adding switch \n' ) 
	s12 = net.addSwitch( 's12' )

	info( '*** Creating links\n' ) 
	net.addLink( h1, s12 ) 
	net.addLink( h2, s12 )
	net.addLink( s12, r1, intfName2='r1-eth0' )
	net.addLink( r1, h3, intfName1='r1-eth1' )

	info( '*** Starting network\n') 
	net.start()
	"This is used to run commands on the hosts"

	info( '*** Configuring hosts\n' )
	r1.cmd('ifconfig r1-eth1 10.0.1.100 netmask 255.255.255.0')
	r1.cmd('echo 0 > /proc/sys/net/ipv4/ip_forward')
	h1.cmd('ip route add default via 10.0.0.100')
	h2.cmd('ip route add default via 10.0.0.101')
	h3.cmd('ip route add default via 10.0.1.100')

	info( '*** Starting xterm on hosts\n' )
	h1.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h1 &') 
	h2.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h2 &')
	h3.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T h3 &')
	r1.cmd('xterm -xrm \'XTerm.vt100.allowTitleOps: false\' -T r1 &')

	info( '*** Running the command line interface\n' ) 
	CLI( net )

	info( '*** Closing the terminals on the hosts\n' ) 
	h1.cmd("killall xterm")
	h2.cmd("killall xterm")
	h3.cmd("killall xterm")
	h4.cmd("killall xterm")

	info( '*** Stopping network' )
	net.stop()

"main Function: This is called when the Python file is run" 
if __name__ == '__main__':
	setLogLevel( 'info' )
	firstNetwork()
```

I executed the `pingall` command in mininet shell to check the connections.

![Untitled](Experiment%203/Untitled%205.png)

❓ Question 8

33% of packets were dropped.

❓ Question 9

- Host 2 pings Host 1 and the router
- Host 1 pings Host 2 and the router
- Host 3 just pings the router
- The Router pings all hosts

❓ Question 10

`255.255.255.0`

❓ Question 11

- `r1-eth0` is `10.0.0.100`
- `r1-eth1` is `10.0.1.100`

❓ Question 12

There are 2 issues in this script.

```python
# Wrong
r1.cmd('echo 0 > /proc/sys/net/ipv4/ip_forward')
# Correct 
r1.cmd('echo 1 > /proc/sys/net/ipv4/ip_forward')

# Wrong
h2.cmd('ip route add default via 10.0.0.101')
# Correct
h2.cmd('ip route add default via 10.0.0.100')
```

Then I executed the mininet.

![Untitled](Experiment%203/Untitled%206.png)

It got fixed!

❓ Question 13

It's 0%.

![Untitled](Experiment%203/Untitled%207.png)

❓ Question 14

There is just one important thing. As you can see in this picture, the MAC addresses of these interfaces (`r1-eth0` and `r1-eth1`) are different. I mean, the incoming packets’ MAC address is different from the source host. In this case, the switch will search for the router to send packets to host 3.

❓ Question 15

- Routing: To connect to another host from a different network, we need to kind of change our IP address. (The address is the second interface of the router)
- Security: This mechanism can hide the hosts’ MAC addresses of a network from a different network.

## Routing

❓ Question 16

I added the default gateway to the router and removed the default gateway from host 3. Therefore, I activate the forwarding.

```bash
# Rotuer
ip route add default via 10.0.1.3

# Host 3 
ip route del default
echo 1 > /proc/sys/net/ipv4/ip_forward
```

❓ Question 17

The initial ICMP was captured by Wireshark in both `eth0` of router and host 3, but host 3 was not able to answer this packet.

![Untitled](Experiment%203/Untitled%208.png)

❓ Question 18

To fix this issue, we can set the default gateway in host 3 so that it can answer the packets.

```bash
# Host 3
ip route add default via 10.0.1.100
```

![Untitled](Experiment%203/Untitled%209.png)