# Final Question

Mohammad Amin Ghasvari - 97521432

## Part I (`ping 78.39.205.100`)

| No. | Eth src | Eth dest | L3-Type | L4-Proto | IP src | IP dest | content |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 11:11 | FF:FF | ARP | - | 78.39.205.100 | 78.39.205.101 | Who has IP dest tell IP src |
| 2 | 22:11 | 11:11 | ARP | - | 78.39.205.101 | 78.39.205.100 | Target MAC, IP |
| 3 | 11:11 | 22:11 | - | ICMP | 78.39.205.100 | 78.39.205.101 | TTL, Type, ID |
| 4 | 22:11 | 11:11 | - | ICMP | 78.39.205.101 | 78.39.205.100 | Reply |

`FF:FF` means broadcast.

`0x0806` is the ARP type.

`0x0800` is the IPv4.

`0x08` is the Echo.

<aside>
💡 Note: the ICMP could be both in layer 4 and 3.

</aside>

## Part II (`traceroute -q 1 www.iust.ac.ir`)

| No. | Eth src | Eth dest | L3-Type | L4-Proto | IP src | IP dest | content |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | 11:11 | FF:FF | ARP | - | 78.39.205.100 | 78.39.205.102 | Who has IP dest tell IP src |
| 2 | 33:11 | 11:11 | ARP | - | 78.39.205.102 | 78.39.205.100 | Target IP src, IP dest, MAC |
| 3 | 11:11 | 33:11 | IPv4 | UDP | 78.39.205.100 | 78.39.205.102 | Query (www.iust.ac.ir), Type (A) |
| 4 | 33:11 | 11:11 | IPv4 | UDP | 78.39.205.102 | 78.39.205.100 | Query (www.iust.ac.ir), Type (A), IP (78.39.205.194) |
| 5 | 11.:11 | FF:FF | ARP | - | 78.39.205.100 | 78.39.205.126 | Who has IP dest tell IP src |
| 6 | 44:22 | 11:11 | ARP |  | 78.39.205.126 | 78.39.205.100 |  |
| 7 | 11:11 | 44:22 | IPv4 | ICMP | 78.39.205.100 | 78.39.205.194 |  |
| 8 | 44:22 | 11:11 | IPv4 | ICMP | 78.39.205.194 | 78.39.205.100 | TTL Exceeded |
| 9 | 11:11 | 55:11 | IPv4 | ICMP | 78.39.205.100 | 78.39.205.194 | Ping (TTL=2) |
| 10 | 55:11 | FF:FF | ARP | - | 78.39.205.254 | 78.39.205.194 | Who has IP dest tell IP src |
| 11 | 55:22 | 55:11 | ARP | - | 78.39.205.194 | 78.39.205.254 |  |
| 12 | 55:11 | 55:22 | IPv4 | ICMP | 78.39.205.100 | 78.39.205.194 | Ping (TTL=1) |
| 13 | 55:22 | 44:22 | IPv4 | ICMP | 78.39.205.194 | 78.39.205.100 | TTL Exceeded |
| 14 | 44:22 | 11:11 | IPv4 | ICMP | 78.39.205.194 | 78.39.205.100 | TTL Exceeded |