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

# 101 - 2

## The boot process

1. Motherboard firmware does a PowerOnSelfTest.
2. Motherboard loads the bootloader.
3. Bootloader loades the Linux Kernel.
4. Kernel loads and prepares the system, and runs `init` program.
5. `init` program starts the services or other applications.

### BIOS

The motherboard itself has firmware; the older version of this firmware was called BIOS.

### UEFI

Nowadays, we have UEFI (Unified Extensible Firmware Interface). UEFI has a separate partition `/boot`. They're modern and fantasy.

EFI System partition or ESP is FAT and mounted on `/boot/efi`. Bootloader files has `.efi` extentions.

> Check `/sys/firmware/efi` to see if you're using UEFI or not.

### Bootloader

The bootloader initialize the minimum hardware needed to boot your system and then scans, finds, and run the OS.

<aside>
💡 Grub is a bootloader.
</aside>

### Kernel

It is the main and core component of your operating system. It is the LINUX itself. Kernel needs some initial information to to start. Those are stored in `initramfs`.

## Logs

### Kernel Ring Buffer

When kernel is loading, there is no space to write logs yet. The kernel uses the Kernel Ring Bugger to remembers and stores these logs. After loading, They're also stored in `/var/log/boot` (for debian) and `/var/log/boot.log` (for redhat).

### dmesg

`dmesg` shows the full data from the Kernel Ring Buffer up to now. But the `/var/log/dmesg` shows the data during the boot. The Kernel is continuously logging its messages to `dmesg`.


### journalctl

It stores the `systemd` logs.

```bash
# To see the kernel logs
journalctl -k

# Boot logs
journalctl -b
```

## init

There are lots of services and daemons running in our operating system. The kernel runs `init` program and then `init` is responsible for launghing those services. 

> **Note:** having a separate program like `init` is a good architecture because it avoids crashing.

init systems:
- SysVinit: It is based on the unix systems and not used much.
- systemd
- upstart: The project started by canonical (The company behind ubuntu) and was canceled.

```bash
# To see your init system
which init 
# Read the link
ls -ltrh /usr/sbin/init
# Or 
readlink -f /usr/sbin/init

# Using process to find out init system
ps -p 1
pstree | less
```

### systemd

`systemd` is a init system. `systemd` work with some units.
The units priority is
1. `/etc/systemd/system/`
2. `/run/systemd/system/`
3. `/usr/lib/systemd/system/`

You can also read this page: [systemctl in linux](Systemctl.md)

```bash
sudo systemctl is-system-running
sudo systemctl --failed
```

#### Units

```bash
# To see the units
sudo systemctl list-units
```

There are 12 types of units, the services are the most important one. The `target`s consist of some services and by loading them all of its services is going to run.

```bash
# To show just a type of units
sudo systemctl list-units --type=target

# Get the default target
sudo systemctl get-default
```

All of these units are stored in files.

```bash
# List the unit files
sudo systemctl list-unit-files

# To cat the unit file.
# In my case it was in `/lib/systemd/system/cups.service`
sudo systemctl cat cups
sudo systemctl cat graphical.target
```

To enable and run these unit files (Services).

```bash
# Status of service
sudo systemctl status sshd

# Start
sudo systemctl start sshd

# Stop
sudo systemctl stop sshd

# Automatically start a service after boot
sudo systemctl enable sshd
```

#### Journalctl

```bash
# Show everything
sudo journalctl
sudo journalctl --no-pager
sudo journlactl -n 100
# Show a range of time
sudo journlactl -S -1d
sudo journlactl -S -1M
# Show from the end
sudo journlactl -xe
# Just a specific type
sudo journlactl -i ntp
# Using process id to filter
sudo journlactl _PID=1234
```

### SysV

The services are located in `/etc/init.d` in systems using SysV. These are some bash scripts files.

```bash
/etc/init.d/ntpd start
/etc/init.d/ntpd status
/etc/init.d/ntpd stop
/etc/init.d/ntpd restart
```

# Resources 

- [Linux1st](https://linux1st.com/)