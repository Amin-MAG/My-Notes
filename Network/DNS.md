# DNS (Domain Name System)

DNS or Domain name servers change the human-readable path or domain to IP addresses. A DNS resolver can be in ISP or popular DNS providers (Google/Cloudflare). If a DNS resolver can not find the domain name in its cache, It will ask authoritative nameserver. When we update a domain's DNS record, we are updating its authoritative nameserver.

## Domain Hierarchy

Domains are organized in a hierarchical tree structure, with the root at the top, followed by top-level domains (TLDs), second-level domains, and subdomains.

## Domain Name Components

Understanding the components of a domain name, such as top-level domain (TLD), second-level domain (SLD), and subdomain.

### Root, Domain, and Name servers

But to find the name server we need to ask it from root server. Root server itself forward this request to the domain server. Finally, the domain server forward the request to the authoritative nameserver that is responsible for the domain name.

## DNS Records

Common types of DNS records include:
- **A Record:** Maps a domain to an IPv4 address.
- **AAAA Record:** Maps a domain to an IPv6 address.
- **CNAME Record:** Alias of one domain to another.
- **MX Record:** Specifies mail servers responsible for receiving emails.
- **NS Record:** Identifies authoritative DNS servers for a domain.
- **PTR Record:** Used for reverse DNS lookup.
- **SOA Record:** Contains information about the domain and the zone.
- **TXT Record:** Holds human-readable information.
- **SRV Record**: points to a server and a service by including a port number.

## DNS Resolution Process

The process by which a DNS resolver translates a domain name into an IP address involves querying authoritative DNS servers.

## Recursive and Iterative Queries

- **Recursive Query:** A DNS resolver requests information from other DNS servers until it receives a complete answer.
- **Iterative Query:** A DNS resolver queries other DNS servers one at a time, making subsequent queries based on the previous responses.

## Root DNS Servers

There are 13 root DNS servers globally that store the information about top-level domains.

## TTL (Time to Live)

Specifies the duration for which a DNS record is considered valid. After this time, the resolver needs to query the authoritative DNS server again.

## DNS Caching

DNS resolvers cache responses to reduce the need for repeated queries.

## DNS Zone

A contiguous portion of the DNS domain name space served by a specific authoritative DNS server.

##  Forwarders

DNS servers that are used to forward queries for domains outside their authoritative zones.

## DNSSEC (DNS Security Extensions)

A suite of extensions to DNS to add an additional layer of security by signing DNS data.

## Anycast DNS

A routing technique where the same IP address is assigned to multiple servers, and the request is routed to the nearest one.

## Dynamic DNS (DDNS)

Allows the automatic update of DNS records, commonly used in situations where the IP address of a host may change regularly.

## Public DNS Services

Familiarity with public DNS services such as Google DNS, OpenDNS, and Cloudflare DNS.

## DNS Tools

Using tools like `nslookup` or `dig` for DNS queries and troubleshooting.

## DNS Resolvers and Authoritative Servers

Understanding the difference between DNS resolvers (recursive servers) and authoritative DNS servers.

## Primary and Secondary DNS Servers

Configuring primary and secondary DNS servers for redundancy and load balancing.

## DNS over HTTPS (DoH) and DNS over TLS (DoT)

Secure methods of DNS communication to protect against eavesdropping and man-in-the-middle attacks.

##  DNS Issues

Troubleshooting common DNS problems, such as misconfigurations, DNS spoofing, and cache poisoning.


