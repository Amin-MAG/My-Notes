# Network

## RTT or Round Trip Time

Round-trip time (RTT) is the duration, measured in milliseconds, from when a browser sends a request to when it receives a response from a server. It’s a key performance metric for web applications and one of the main factors, along with Time to First Byte (TTFB).

## Network Congestion

The network can handle the transmission of a specific amount of data. If there is more data transmission, it leads to a decrease in performance and an increase in latency. Here are some reasons:

- Heavy Network Traffic
- Network Failure
- Insufficient Bandwidth
- Malware or Cyberattacks

# VPN

## Setup OpenVPN

```bash
# Install the openvpn
sudo apt install openvpn

# Download easy rsa 
wget -P ~/ https://github.com/OpenVPN/easy-rsa/releases/download/v3.0.8/EasyRSA-3.0.8.tgz
tar xvf EasyRSA-3.0.8.tgz
```

In CA:

Change the vars file

```bash
# Make a copy and change the configurations
cp vars.example vars
```

Then

```bash
# Initialize the easy rsa
./easyrsa init-pki

# Build the certificates with no pass
./easyrsa build-ca nopass
```

```bash
# Install the openvpn
sudo apt install openvpn easy-rsa

# Copy easy-rsa directory
sudo make-cadir /etc/openvpn/easy-rsa
```

In the server:

```bash
# Initialize the easy rsa
./easyrsa init-pki

# Generate private key and a certificate
./easyrsa gen-req server nopass

# Copy the certificate file
sudo cp ~/EasyRSA-3.0.8/pki/private/server.key /etc/openvpn/

# Then send your ./pki/reqs/server.req to the CA.
scp ~/EasyRSA-3.0.8/pki/reqs/server.req sammy@your_CA_ip:/tmp
```

In CA:

```bash
# Import it
./easyrsa import-req ./../server.req server

# Sign it in CA
./easyrsa sign-req server server

# Send certificates back to the server
scp pki/issued/server.crt sammy@your_server_ip:/tmp
scp pki/ca.crt sammy@your_server_ip:/tmp
```

Back into the server:

```bash
# Copy certificates to openvpn configs
sudo cp /tmp/{server.crt,ca.crt} /etc/openvpn/

# Generate another kind of keys for more secure connection
./easyrsa gen-dh
openvpn --genkey --secret ta.key
sudo cp ~/EasyRSA-3.0.8/ta.key /etc/openvpn/
sudo cp ~/EasyRSA-3.0.8/pki/dh.pem /etc/openvpn/
```

In the client

```bash
mkdir -p ~/client-configs/keys
chmod -R 700 ~/client-configs

# Generate a key and put it in keys
./easyrsa gen-req client1 nopass
cp pki/private/client1.key ~/client-configs/keys/

# Transfer req to CA
scp pki/reqs/client1.req sammy@your_CA_ip:/tmp
```

On the CA machine

```bash
./easyrsa import-req /tmp/client1.req client1
./easyrsa sign-req client client1

# Send the certificate to the server
scp pki/issued/client1.crt sammy@your_server_ip:/tmp
cp ~/EasyRSA-3.0.8/ta.key ~/client-configs/keys/
sudo cp /etc/openvpn/ca.crt ~/client-configs/keys/
```

## TCP / UDP

# See More

- [VPN](Network/VPN%2009efe.md)
- [Proxy](Proxy.md)
- [Load Balancing](Network/Load-Balancing.md)
- [CDN](CDN.md)
- [Tor](Network/Tor%20662bd.md)
- [Setup a security camera](Network/SecurityCamera.md)
- [Laboratory](Network/Laboratory%20a293b.md)
- [University](Network/University%20bc284.md)


