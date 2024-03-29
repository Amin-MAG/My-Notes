# University

## Transfer delay

`dtrans`(output port to output link)

`dqueue`(the queue back of the port)

`dproc`(processing)

`dprop` () = $LentghOfLink/V_{light}$

$dnodal = dprop + dproc + dqueue + dtrans$

$d_{prop} = Distance/PropagationSpeed$

$d_{trans} = L/R$

## Packet loss

Traffic Intensity = $I =La/R$

It shouldn't be more than 70-90 percent, because it will cause lots of traffic in the queue.

$d_{queue}$ = $(I/1-I).(L/R)$

Increasing $I$ causes increasing $d_{queue}$ and packet loss. So if we want to use retransmission to fix this, it will increases conjunctions.

## Throughput

It means the effective traffic transmission rate. Throughput is based on the bottleneck in the network.

Throughput = $min(Rate_1, Rate_2, ...)$

Utilization = $Rate/Throughput$ 

$X = T_b . S$

$U = X/d$

## Books

Computer Networking: A Top-Down Approach, 7th Edition (First Six Chapters)

[Wireshark Assignment I](Wireshark%20%2066880.md)

[Wireshark Assignment II](Wireshark%20%2035745.md)

[Final Question](Final%20Ques%2054da3.md)