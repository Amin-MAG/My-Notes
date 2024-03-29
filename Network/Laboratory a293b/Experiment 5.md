# Experiment 5

## Overview

In some cases, I used this command to jump into the VM machine from another machine in my local network.

```bash
ssh mininet@192.168.1.10
```

First of all, I sent the files with `scp` command.

```bash
scp -r lab5 mininet@192.168.43.75
```

This folder contains some scripts to measure goodput. To make them executable

```bash
chmod +x tcp/tcpclient tcp/tcpserver udp/udpclient udp/udpserver
```

To check the current system congestion mechanism

```bash
cat /proc/sys/net/ipv4/tcp_congestion_control
# cubic
```

The mechanism was a TCP cube. If you want to change this mechanism to another one until next reboot, you can change the content of this file.

```bash
sudo bash -c 'echo reno > /proc/sys/net/ipv4/tcp_congestion_control'
cat /proc/sys/net/ipv4/tcp_congestion_control
# reno
```

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled.png)

## TCP & UDP Stream

This is the scenario

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%201.png)

To create the scenario, we need to run the python code. (This code is edited)

```python
import inspect
import os

# Needed to patch Mininet's isShellBuiltin module
import sys

# Run commands when you exit the python script
import atexit

# patch isShellBuiltin (suggested by MiniNExT's authors)
import mininet.util

sys.modules['mininet.util'] = mininet.util

# Loads the default controller for the switches

from mininet.node import Controller

# Needed to set logging level and show useful information during script execution.
from mininet.log import setLogLevel, info

# To launch xterm for each node
from mininet.term import cleanUpScreens, makeTerms # for supporting copy/paste

# Provides the mininet> prompt
from mininet.cli import CLI

# Primary constructor for the virtual environment.
from mininet.net import Mininet

# We import the TC-enabled link
from mininet.link import Intf, TCIntf, TCLink

# Variable initialization
net = None
hosts = None

def run():
    " Creates the virtual environment, by starting the network and configuring debug information "
    info('** Creating an instance of Lab5 network topology\n')
    global net
    global hosts

    net = Mininet(intf=TCIntf)

    info('\n** Adding Controller\n')
    net.addController( 'c0' )

    info('\n** Adding Hosts\n')
    h1 = net.addHost('h1', ip='10.10.0.1/24', hostname='h1',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    h2 = net.addHost('h2', ip='10.10.0.2/24', hostname='h2',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    h3 = net.addHost('h3', ip='10.10.1.3/24', hostname='h3',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    r1 = net.addHost('r1', ip='10.10.1.10/24', hostname='r1',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)

    info('\n** Adding Switches\n')
    # Adding switches to the network
    sw1 = net.addSwitch('sw1')
    sw2 = net.addSwitch('sw2')

    info('\n** Creating Links \n')
    link_h1sw1 = net.addLink( h1, sw1)
    link_h2sw1 = net.addLink( h2, sw1)
    link_h3sw2 = net.addLink( h3, sw2)
    link_r1sw1 = net.addLink( r1, sw1, intfName1='r1-eth0')
    link_r1sw2 = net.addLink( r1, sw2, intfName1='r1-eth1')

    info('\n** Modifying Link Parameters \n')
    """
        Default parameters for links:
        bw = None,
                delay = None,
                jitter = None,
                loss = None,
                disable_gro = True,
                speedup = 0,
                use_hfsc = False,
                use_tbf = False,
                latency_ms = None,
                enable_ecn = False,
                enable_red = False,
                max_queue_size = None
    """
    link_r1sw2.intf1.config( bw=10, enable_red=True , enable_ecn=True)

    net.start()

    info( '*** Configuring hosts\n' )
    info('** Executing custom commands\n')
    output = net.nameToNode.keys
    r1.cmd('ifconfig r1-eth0 10.10.0.10 netmask 255.255.255.0')
    r1.cmd('ifconfig r1-eth1 10.10.1.10 netmask 255.255.255.0')
    r1.cmd('echo 0 > /proc/sys/net/ipv4/ip_forward')

    h1.cmd('ip route add default via 10.10.0.10')
    h2.cmd('ip route add default via 10.10.0.10')
    h3.cmd('ip route add default via 10.10.1.10')
    # Space to add commands for configuring routing tables and default gateways

    #Enable Xterm window for every host
    info('** Enabling xterm for hosts only\n')
    # We check if the display is available
    hosts = [ h1, h2, h3, r1 ]
    if 'DISPLAY' not in os.environ:
        error( "Error starting terms: Cannot connect to display\n" )
        return
    # Remove previous (and possible non-used) socat X11 tunnels
    cleanUpScreens()
    # Mininet's function to create Xterms in hosts
    makeTerms( hosts, 'host' )

    # Enable the mininet> prompt
    info('** Running CLI\n')
    CLI(net)

    info( '*** Closing the terminals on the hosts\n' )
    h1.cmd("killall xterm")
    h2.cmd("killall xterm")
    h3.cmd("killall xterm")
    r1.cmd("killall xterm")

    # This command stops the simulation
    net.stop()
    cleanUpScreens()

if __name__ == '__main__':
    # Set the log level on terminal
    setLogLevel('info')

    # Execute the script
    run()
```

- I added some missing IP addresses for h1, h2, and router (r1).
- Consider the same subnet masks for h3 and router.

Then, I ran the code to create the scenario.

```python
python3 lab5_network.py
```

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%202.png)

Using `pingall` command you can check all the connections.

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%203.png)

## Change network configuration

I changed the python code configurations to satisfy the conditions.

```python
# Needed to check for display status
import inspect
import os

# Needed to patch Mininet's isShellBuiltin module
import sys

# Run commands when you exit the python script
import atexit

# patch isShellBuiltin (suggested by MiniNExT's authors)
import mininet.util

sys.modules['mininet.util'] = mininet.util

# Loads the default controller for the switches

from mininet.node import Controller

# Needed to set logging level and show useful information during script execution.
from mininet.log import setLogLevel, info

# To launch xterm for each node
from mininet.term import cleanUpScreens, makeTerms # for supporting copy/paste

# Provides the mininet> prompt
from mininet.cli import CLI

# Primary constructor for the virtual environment.
from mininet.net import Mininet

# We import the TC-enabled link
from mininet.link import Intf, TCIntf, TCLink

# Variable initialization
net = None
hosts = None

def run():
    " Creates the virtual environment, by starting the network and configuring debug information "
    info('** Creating an instance of Lab5 network topology\n')
    global net
    global hosts

    net = Mininet(intf=TCIntf)

    info('\n** Adding Controller\n')
    net.addController( 'c0' )

    info('\n** Adding Hosts\n')
    h1 = net.addHost('h1', ip='10.10.0.1/24', hostname='h1',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    h2 = net.addHost('h2', ip='10.10.0.2/24', hostname='h2',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    h3 = net.addHost('h3', ip='10.10.1.3/24', hostname='h3',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)
    r1 = net.addHost('r1', ip='10.10.1.10/24', hostname='r1',  privateLogDir=True, privateRunDir=True, inMountNamespace=True, inPIDNamespace=True, inUTSNamespace=True)

    info('\n** Adding Switches\n')
    # Adding switches to the network
    sw1 = net.addSwitch('sw1')
    sw2 = net.addSwitch('sw2')

    info('\n** Creating Links \n')
    link_h1sw1 = net.addLink( h1, sw1)
    link_h2sw1 = net.addLink( h2, sw1)
    link_h3sw2 = net.addLink( h3, sw2)
    link_r1sw1 = net.addLink( r1, sw1, intfName1='r1-eth0')
    link_r1sw2 = net.addLink( r1, sw2, intfName1='r1-eth1')

    info('\n** Modifying Link Parameters \n')
    """
        Default parameters for links:
        bw = None,
                delay = None,
                jitter = None,
                loss = None,
                disable_gro = True,
                speedup = 0,
                use_hfsc = False,
                use_tbf = False,
                latency_ms = None,
                enable_ecn = False,
                enable_red = False,
                max_queue_size = None
    """
    # link_r1sw2.intf1.config( bw=10, enable_red=True , enable_ecn=True)
    link_r1sw2.intf1.config( bw=3, enable_red=True , enable_ecn=True)

    net.start()

    info( '*** Configuring hosts\n' )
    info('** Executing custom commands\n')
    output = net.nameToNode.keys
    r1.cmd('ifconfig r1-eth0 10.10.0.10 netmask 255.255.255.0')
    r1.cmd('ifconfig r1-eth1 10.10.1.10 netmask 255.255.255.0')
    r1.cmd('echo 1 > /proc/sys/net/ipv4/ip_forward')

    h1.cmd('ip route add default via 10.10.0.10')
    h2.cmd('ip route add default via 10.10.0.10')
    h3.cmd('ip route add default via 10.10.1.10')
    # Space to add commands for configuring routing tables and default gateways

    #Enable Xterm window for every host
    info('** Enabling xterm for hosts only\n')
    # We check if the display is available
    hosts = [ h1, h2, h3, r1 ]
    if 'DISPLAY' not in os.environ:
        error( "Error starting terms: Cannot connect to display\n" )
        return
    # Remove previous (and possible non-used) socat X11 tunnels
    cleanUpScreens()
    # Mininet's function to create Xterms in hosts
    makeTerms( hosts, 'host' )

    # Enable the mininet> prompt
    info('** Running CLI\n')
    CLI(net)

    info( '*** Closing the terminals on the hosts\n' )
    h1.cmd("killall xterm")
    h2.cmd("killall xterm")
    h3.cmd("killall xterm")
    r1.cmd("killall xterm")

    # This command stops the simulation
    net.stop()
    cleanUpScreens()

if __name__ == '__main__':
    # Set the log level on terminal
    setLogLevel('info')

    # Execute the script
    run()
```

I checked the connection between the hosts with `pingall` command.

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%204.png)

### ❓ Question 1

I started a UDP client on host 1 and a UDP server on host 3.

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%205.png)

The loss is zero and the goodput is `100 kbit/s`

### ❓ Question 2

Then, I start the same scenario with different amount rates.

### Rate=1Mgb/s

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%206.png)

$loss=99\%$

$goodput=(56429 \times (1-0.99))=564.29$ 

### Rate=10Mgb/s

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%207.png)

$loss=99.3\%$

$goodput=(56429 \times (1-0.993))=395.003$

### Rate=100Mgb/s

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%208.png)

$loss=99.5\%$

$goodput=(56429 \times (1-0.995))=282.145$

### Rate=1Gb/s

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%209.png)

$loss=99.5\%$

$goodput=(56429 \times (1-0.995))=282.145$

As you can see, when $rate > 10Mbps$ the loss is higher.

```python
link_r1sw2.intf1.config( bw=3, enable_red=True , enable_ecn=True)
```

This limitation is due to the above line.

I started a TCP server on host 3 that was listening on port 10001

### ❓ Question 3

This was about `8300kbps`.

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%2010.png)

Now we limit the bandwidth of `eth1` of the router to `3Mbps`

### ❓ Question 4

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%2011.png)

- The header of UDP = 8 Byte
- The header of IP = 20 Byte
- The header of ethernet = 14 Byte
- The application content = 1000 Byte

Totally, It is 1042 Bytes.

### ❓ Question 5

$\frac{Bandwidth \times 1000}{1042} = 2880$

### ❓ Question 6

I launch a UDP server on host 3 that was listening on port 10000. Then, I created a client on host 1 that was sending data at the rate of `100kbps`

The goodput turns out to be %100 or `100kbps` and the loss was 0.

### ❓ Question 7

I did the exact same thing for `10Mbps` and `3Mbps`.

At `3Mbps`, I had a %29 loss and `2990kbps` goodput.

At `10Mbps`, I had a %77 loss and `9982kbps` goodput.

### ❓ Question 8

It is expected to be 1514 Byte.

### ❓ Question 9

$\frac{Bandwidth \times 1448}{1514} = 2869$

### ❓ Question 10

I launched a TCP client on host 1.

![Untitled](Experiment%205%20ceee690c813340a0afea7e3170b5b131/Untitled%2012.png)