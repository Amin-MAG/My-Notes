# Latency Numbers

![](Operating-Systems/LatencyNumbers.png)

- **CPU registers access: Less than 1 ns**  
- **L1 and L2 caches: 1-10 ns**  
- **L3 caches access, or RAM access: 10-100 ns**
- **Linux system calls: 100-1000 ns**
- **Context switching between Linux threads, Send 1K bytes over 1 Gbps network: 1-10 us** 
- **Processing an HTTP request in Nginx, Read an 8K page from SSD: 10-100 us**  
- **Database insert operation, Intra-Zone network round trip in modern cloud providers, Redis get operation: 100-1000 us**
- **Seek time of HDD: 1-10 ms**
- **Send packet CA->Netherlands->CA: 10-100 ms**  
- **TLS Handshake: 100-1000 ms**
- **Retry/refresh internal or Sending 1Gb data from : 1-10s**  

# Resources

- [Latency numbers you should know](https://blog.bytebytego.com/p/ep22-latency-numbers-you-should-know)
- [Latency numbers you should know- YouTube video](https://www.youtube.com/watch?v=FqR5vESuKe0)