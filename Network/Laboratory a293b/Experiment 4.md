# Experiment 4

## Overview

In this experiment, we are going to connect an isolated network to an external network.

![Untitled](Experiment%203c589/Untitled.png)

To create this scenario

```python
#!/usr/bin/python
""" This example shows how to create a Mininet object and add nodes to
it manually.  """

"Importing Libraries"
from mininet.net import Mininet
from mininet.node import Controller
from mininet.cli import CLI
from mininet.log import setLogLevel, info
from mininet.link import Intf

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
	s3 = net.addSwitch( 's3' )

	info( '*** Creating links\n' ) 
	net.addLink( h1, s12 ) 
	net.addLink( h2, s12 )
	net.addLink( s12, r1, intfName2='r1-eth0' )
	net.addLink( r1, h3, intfName1='r1-eth1', intfName2='h3-eth0' )
	net.addLink( h3, s3, intfName1='h3-eth1')

	info( '*** Starting network\n') 
	net.start()
	"This is used to run commands on the hosts"

	info( '*** Configuring hosts\n' )
	r1.cmd('ifconfig r1-eth1 10.0.1.100 netmask 255.255.255.0')
	h3.cmd('ifconfig h3-eth1 0.0.0.0')
	r1.cmd('ip route add default via 10.0.1.3')
	h1.cmd('ip route add default via 10.0.0.100')
	h2.cmd('ip route add default via 10.0.0.100')
	h3.cmd('ip route add 10.0.0.0/24 via 10.0.1.100')
	r1.cmd('echo 1 > /proc/sys/net/ipv4/ip_forward')
	h3.cmd('echo 1 > /proc/sys/net/ipv4/ip_forward')

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

## Part I - Bridging

### ❓ Question 1

 Since we want the clients to have the same IP address in the internet we can use NAT to translate packets from VM to physical network.

### ❓ Question 2

I started capturing the interfaces using Wireshark. Then I sent an ICMP packet with `ping` packet.

![Untitled](Experiment%203c589/Untitled%201.png)

As you can see, The IP address is `10.0.2.15`. 

![Untitled](Experiment%203c589/Untitled%202.png)

The interface is `eth-0`.

To connect the `r1` to the internet we need to perform bridging. To connect the interface of VM to the switch s3.

```bash
# To add 
sh ovs-vsctl add-port s3 eth0

# To show
sh ovs-vsctl show
```

![Untitled](Experiment%203c589/Untitled%203.png)

Now `eth0` is the interface of the `s3`.

```bash
# To clear the IP address for `eth0`
sudo ip addr flush eth0
```

To assign appropriate IP address with subnet mask you can use `dhclient` command.

```bash
dhclient h3-eth1
```

![Untitled](Experiment%203c589/Untitled%204.png)

Now it assigned `10.0.2.16/24` to this interface. Then, I tried sending packets and establishing a connection to the network.

![Untitled](Experiment%203c589/Untitled%205.png)

## Part II - NAT

### ❓ Question 3

When I sent a packet to the domain and here was the result.

![Untitled](Experiment%203c589/Untitled%206.png)

The DNS was not set so the result was not successful.

### ❓ Question 4

Then, I used the IP address to send the packet.

![Untitled](Experiment%203c589/Untitled%207.png)

The problem was the NAT, but it couldn’t translate the packets.

### ❓ Question 5

```bash
# To fix the router
iptables -t nat -A POSTROUTING -o h3-eth1 -j MASQURATE
```

![Untitled](Experiment%203c589/Untitled%208.png)

The I could ping google. To enable NAT you need to change the source of the sent packets form `h1` and the send them to the internet.

### ❓ Question 6

To find the source of the packets, It uses the IP address in IP Header and the port number in Transport Layer Header.