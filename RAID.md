# RAID

It stands for "Redundant Array of Independent Disks". It is a technology to improve the reliability, availability, and performance. RAID uses combination of multiple physical hard drives and various techniques for organizing and managing data across those hard drives. The primary goal of the RAID is redundancy and/or performance.

## Redundancy

Redundancy refers to practice of duplicating or creating an additional copies of the data to ensure its availability and integrity. Redundancy is a key concept in data protection and fault tolerance.

## Common RAID Levels

### RAID 0 - Striping

This RAID level splits the data into blocks and spread them across multiple drives. It offers **improved performance**, because each drive stores a portion of data which as a result, data can read from or written to all drives simultaneously. Even though, if a hard drive fails, all data is lost. So it provides **no Redundancy**.

### RAID 1 - Mirroring

In this configuration, data is duplicated across two or multiple hard drives to provide redundancy. So if a hard drive fails, data is still accessible. **RAID 1 offers data protection but doesn't necessarily offers improve performance.** You always get the 50% capacity of your drives.

### RAID 5 

RAID 5 stripes data like RAID 0 but also includes parity information. Parity can be used in reconstructing data in case of a drive failure. **It offers a balance between performance and redundancy an is often a good fit for small to  medium-sized businesses.** You always get the 75% capacity of your drives. Calculating the parity make the write operation comparably slow, Also, reconstructing a lost data take considerable amount of computation (which is acceptable).

### RAID 6

It is similar to RAID 5, but it includes two sets of parity information. This allows for two drive failures without data loss. **It provides a higher level of redundancy than RAID 5.**

### RAID 10 - RAID 1+0

RAID 10 is a combination of RAID 1 and RAID 0. It mirrors the data like in RAID 1 and then stripes the mirrored data like RAID 0. **It provides both redundancy and improved performance, but it requires a minimum of four drives.** You always get the 50% capacity of your drives.

### RAID 50  - RAID 5+0

RAID 50 combines the striping of RAID 0 with the distributed parity of RAID 5. **It is suitable for environments that require both performance and redundancy.**

### RAID 60 - RAID 6+0

RAID 60 is a combination of RAID 6 and RAID 0. **It offers better redundancy and performance than RAID 50.**

## RAID 0 VS Sharding

the key difference between RAID 0 and sharding is their primary purpose and the level of redundancy they offer. RAID 0 is focused on performance and provides no data redundancy, making it unsuitable for data protection. Sharding, on the other hand, is focused on data distribution and scalability, enabling the management of large datasets across multiple servers, but it does not inherently provide redundancy either.