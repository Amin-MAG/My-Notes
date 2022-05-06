# LPIC I

# 101 - 1

## Operating System

The computer is all just some hardware. An operating system is software that manages hardware and its resources. It allocates resources to the applications.

## Firmware

Firmware is something between hardware and software. It enables that hardware to communicate to the operating system or other hardware.

The motherboard itself has firmware; the older version of this firmware was called BIOS. Nowadays, we have UEFI (Unified Extensible Firmware Interface). UEFI has a separate partition `/boot`. They're modern and fantasy.

## Peripheral Devices

### PCI (Peripheral Component Interconnect)

It enables us to add the hardware boards to the motherboard.

### Gpio (General Purpose Input Output)

## sysfs

It is a pseudo-file system provided by the Linux kernel. It has information about:
- kernel subsystems
- hardware devices
- associated device drivers

> pseudo-file means that they're not on the disk‌‌ but in kernel memory.

To see `sysfs` files, you can enter `sys` directory
```bash
cd /sys/

# Network setting like IP forwarding
cat /sys/net/ipv4/ip_forwarding
```

## udev

`udev` or userspace `/dev` is a device manager for the Linux kernel. It is also a pseudo-file system. We usually use this `/dev` to manage our devices.

To see all devices:

```bash
cd /dev
```

## dbus

D-bus is a message bus system allowing applications to talk to one another. 

## proc

`proc` is just another pseudo-file system. `proc` is where the kernel keeps its setting and properties.
You can see all of these:
- IRQ - Interrupt requests
- I/O Port
- DMA 
- Processes
- Network Settings
- ...

Some examples:

```bash
# Details about cpu
cat /proc/cpuinfo

# Details about memory
cat /proc/meminfo
```

> The numbers represent the processes.

## ls commands

- `lsusb` - usb devices
- `lspci` - devices connected to computer
- `lsblk` - block devices RAM/ROM/Disk
- `lshw` - 

## Loadable Kernel Modules
Linux like any other OS needs drivces. In Linux, most of these drivers are built-in. `.ko` files are object files used to extend the kernel of the Linux distribution.

Here are some Modules Loading commands:

```bash
# To list all modules
lsmod

# To remote a module
sudo rmmod tls

# to insert a module
sudo insmod <ADDRESS_OF_MODULE>
sudo modprobe tls
```

# Resources 

- [Linux1st](https://linux1st.com/)