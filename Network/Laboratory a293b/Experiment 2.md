# Experiment 2

✏️  M.Amin Ghasvari  (97521432)

## Part 1 - Get started

Like the previous experiment, I create the hosts and switches with the python code.

In some cases, I used this command to jump into the VM machine from another machine in my local network.

```bash
ssh mininet@192.168.1.10
```

❓ Question 1

Here is the code that I used for creating the hosts and switches.

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
    h1 = net.addHost('h1', ip='10.10.14.1/24')
    h2 = net.addHost('h2', ip='10.10.24.2/24')
    h3 = net.addHost('h3', ip='10.10.34.3/24')
    h4 = net.addHost('h4', ip='10.10.14.4/24')

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

    h4.cmd('ip addr add 10.10.24.4/24 dev h4-eth1')
    h4.cmd('ip addr add 10.10.34.4/24 dev h4-eth2')
    h4.cmd('echo 1 > /proc/sys/net/ipv4/ip_forward')

    info('*** Starting network\n')
    net.start()

    info( '*** Adding Gateways\n')
    h1.cmd('ip route add default via 10.10.14.4')
    h2.cmd('ip route add default via 10.10.24.4')
    h3.cmd('ip route add default via 10.10.34.4')

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

## Part 2 - About Man In Middle Scenario

In this experiment, each one of the hosts has a kind of label.

![Untitled](Experiment%202/Untitled.png)

- Host2 is Bob.
- Host1 is the Bank.
- Host4 is the router to connect to the Bank.
- Host3 is the man in middle attacker.

## Part 3

To disable RPF (Reverse Path Filtering), I ran these scripts in the Host4

```bash
echo 0 > /proc/sys/net/ipv4/conf/all/rp_filter
echo 0 > /proc/sys/net/ipv4/conf/h4-eth0/rp_filter
echo 0 > /proc/sys/net/ipv4/conf/h4-eth1/rp_filter
echo 0 > /proc/sys/net/ipv4/conf/h4-eth2/rp_filter
```

To enable port forwarding on host3

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

## Part 4 - Creating Man In The Middle Scenario

Here I created the man in middle scenario.

❓ Question 2

I used the `ip tables` command to forward the specific packets to the host4 (attacker).

```bash
iptables -t nat -A PREROUTING -p icmp -s 10.10.24.2 -d 10.10.14.1 -j DNAT --to 10.10.34.3
```

This command will create a rule to forward each `icmp` packet from `10.10.24.2` to the `10.10.34.3` if it goes to `10.10.14.1`.

I opened the Wireshark in host3 and `ping` the bank from bob’s computer and here is the result in host3

![Untitled](Experiment%202/Untitled.jpeg)

As you can see, I could see the packets coming to the host3.

Then, I had to create 2 rules; First one before leaving host3 and another one after leaving the host3.

```python
iptables -t nat -A PREROUTING -p icmp -s 10.10.24.2 -d 10.10.34.3 -j DNAT --to 10.10.14.1
iptables -t nat -A POSTROUTING -p icmp -o h3-eth0 -s 10.10.24.2 -j SNAT --to 10.10.34.3
```

> Consider that we should enable the port forwarding for host3
> 

I needed to change the source IP address because of the rule I defined in the router. Otherwise, The source was `10.10.24.2` and the destination was `10.10.14.1` just as the rule we defined! 

Host3 could send packets from Bob to the Bank and from the Bank to Bob. 

![Untitled](Experiment%202/Untitled%201.jpeg)

As you can see, I sent a packet using the `ping` command to the Bank from Bob’s computer. First, The packet was sent to host3 and then to the Bank. The response from the Bank was also sent to the host3 first and then from host3 to Bob’s computer.

❓ Question 3

So we need to hide the IP address of host3 and replace it with Bob’s IP address. I added another rule to the router to change the source IP address of packets from host3 going to the Bank.

```bash
iptables -t nat -A POSTROUTING -p icmp -s 10.10.34.3 -j SNAT --to 10.10.24.2
```

![Untitled](Experiment%202/Untitled%202.jpeg)

As you can see, There is no clue of host3!

❓ Question 4

The attacker might even change the routing table to redirect the traffic on the victim network to the node he controls. The computers on the victim network might not even be aware of the forged route and continue to communicate.

❓ Question 5

We should encrypt our data and use certificates (TLS/SSL Protocols) to prevent this security issue. Then, the host3 only has the encrypted data and can not do anything. RPF is also useful for recognization and dropping the packet.