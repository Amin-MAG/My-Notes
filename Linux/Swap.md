# Swap

The swap is like an extended memory. The kernel will page memory and then it can execute a program that needs more memory than the system has. (Although it is slow)

Swap can free your memory and write them on the disk so that you can do much more than before. Also, it allocates memory to the data on the swap when the user wants them back. It is much slower. Swap can be

- A separate partition. (It is recommended)

- A special file in the `/` partition.

- A `zram` - It specifies a disk part in the ram for `zram` and It will compress and decompress the data and save it in that part. The data written to the memory is usually well compressible.

```bash
# Show available memory amount (physical and swap)
free -h 
top

# To see the swap info
swapon

# zram
zramctl
```
