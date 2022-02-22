# Network

## Forward Proxy

A forward proxy, often called a proxy, proxy server, or web proxy, is a server that sits in front of a group of client machines. When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.

For example, let’s name 3 computers involved in a typical forward proxy communication:

- A: This is a user’s home computer
- B: This is a forward proxy server
- C: This is a website’s origin server (where the website data is stored)

![https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg)

Why would anyone add this extra middleman to their Internet activity? There are a few reasons one might want to use a forward proxy:

- **To avoid state or institutional browsing restrictions** - Some governments, schools, and other organizations use firewalls to give their users access to a limited version of the Internet. A forward proxy can be used to get around these restrictions, as they let the user connect to the proxy rather than directly to the sites they are visiting.
- **To block access to certain content** - Conversely, proxies can also be set up to block a group of users from accessing certain sites. For example, a school network might be configured to connect to the web through a proxy which enables content filtering rules, refusing to forward responses from Facebook and other social media sites.
- **To protect their identity online** - In some cases, regular Internet users simply desire increased anonymity online, but in other cases, Internet users live in places where the government can impose serious consequences to political dissidents. Criticizing the government in a web forum or on social media can lead to fines or imprisonment for these users. If one of these dissidents uses a forward proxy to connect to a website where they post politically sensitive comments, the [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) used to post the comments will be harder to trace back to the dissident. Only the IP address of the proxy server will be visible.

### Resources

[](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)

## Reverse Proxy

A reverse proxy is a server that sits in front of one or more web servers, intercepting requests from clients. This is different from a forward proxy, where the proxy sits in front of the clients. With a reverse proxy, when clients send requests to the origin server of a website, those requests are intercepted at the [network edge](https://www.cloudflare.com/learning/serverless/glossary/what-is-edge-computing/) by the reverse proxy server. The reverse proxy server will then send requests to and receive responses from the origin server.

The difference between a forward and reverse proxy is subtle but important. A simplified way to sum it up would be to say that a forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. On the other hand, a reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.

Once again, let’s illustrate by naming the computers involved:

- D: Any number of users’ home computers
- E: This is a reverse proxy server
- F: One or more origin servers

![https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg)

Below we outline some of the benefits of a reverse proxy:

- **[Load balancing](https://www.cloudflare.com/learning/cdn/cdn-load-balance-reliability/)** - A popular website that gets millions of users every day may not be able to handle all of its incoming site traffic with a single origin server. Instead, the site can be distributed among a pool of different servers, all handling requests for the same site. In this case, a reverse proxy can provide a load balancing solution which will distribute the incoming traffic evenly among the different servers to prevent any single server from becoming overloaded. In the event that a server fails completely, other servers can step up to handle the traffic.
- **Protection from attacks** - With a reverse proxy in place, a web site or service never needs to reveal the IP address of their origin server(s). This makes it much harder for attackers to leverage a targeted attack against them, such as a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/). Instead the attackers will only be able to target the reverse proxy, such as Cloudflare’s [CDN](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/), which will have tighter security and more resources to fend off a cyber attack.
- **[Global Server Load Balancing](https://www.cloudflare.com/learning/cdn/glossary/global-server-load-balancing-gslb/) (GSLB)** - In this form of load balancing, a website can be distributed on several servers around the globe and the reverse proxy will send clients to the server that’s geographically closest to them. This decreases the distances that requests and responses need to travel, minimizing load times.
- **Caching** - A reverse proxy can also [cache](https://www.cloudflare.com/learning/cdn/what-is-caching/) content, resulting in faster performance. For example, if a user in Paris visits a reverse-proxied website with web servers in Los Angeles, the user might actually connect to a local reverse proxy server in Paris, which will then have to communicate with an origin server in L.A. The proxy server can then cache (or temporarily save) the response data. Subsequent Parisian users who browse the site will then get the locally cached version from the Parisian reverse proxy server, resulting in much faster performance.
- **SSL encryption** - [Encrypting](https://www.cloudflare.com/learning/ssl/what-is-encryption/) and decrypting [SSL](https://www.cloudflare.com/learning/security/glossary/what-is-ssl/) (or [TLS](https://www.cloudflare.com/learning/security/glossary/transport-layer-security-tls/)) communications for each client can be computationally expensive for an origin server. A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.

### Resources

[](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)

## 

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

## Other pages

[University](Network%2008880/University%20bc284.md)

[VPN](Network%2008880/VPN%2009efe.md)

[Tor](Network%2008880/Tor%20662bd.md)