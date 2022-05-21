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
> 

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

The D-bus is a message bus system allowing applications to talk to one another.

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
> 

## ls commands

- `lsusb` - usb devices
- `lspci` - devices connected to computer
- `lsblk` - block devices RAM/ROM/Disk
- `lshw` -

## Loadable Kernel Modules

Linux like any other OS needs drives. In Linux, most of these drivers are built-in. `.ko` files are object files used to extend the kernel of the Linux distribution.

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

---

# 101 - 2

## The boot process

1. Motherboard firmware does a PowerOnSelfTest.
2. Motherboard loads the bootloader.
3. Bootloader loads the Linux Kernel.
4. Kernel loads and prepares the system and runs `init` program.
5. `init` program starts the services or other applications.

### BIOS

The motherboard itself has firmware; the older version of this firmware was called BIOS.

### UEFI

Nowadays, we have UEFI (Unified Extensible Firmware Interface). UEFI has a separate partition `/boot`. They're modern and fantasy.

EFI System partition or ESP is FAT and mounted on `/boot/efi`. Bootloader files has `.efi` extensions.

> Check /sys/firmware/efi to see if you're using UEFI or not.
> 

### Bootloader

The bootloader initializes the minimum hardware needed to boot your system and then scans, finds, and runs the OS.

<aside>
💡  Grub is a bootloader.

</aside>

### Kernel

It is the main and core component of your operating system. It is the LINUX itself. The kernel needs some initial information to start. Those are stored in `initramfs`.

## Logs

### Kernel Ring Buffer

When the kernel is loading, there is no space to write logs yet. The kernel uses the Kernel Ring Bugger to remember and store these logs. After loading, They're also stored in `/var/log/boot` (for Debian) and `/var/log/boot.log` (for RedHat).

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

There are lots of services and daemons running in our operating system. The kernel runs `init` program and then `init` is responsible for launching those services.

> Note: having a separate program like init is a good architecture because it avoids crashing.
> 

init systems:

- SysVinit: It is based on the Unix systems and not used much.
- systemd
- upstart: The project was started by canonical (The company behind Ubuntu) and was canceled.

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

You can also read this page: [systemctl in linux](https://www.notion.so/amin-mag/Systemctl.md)

```bash
sudo systemctl is-system-running
sudo systemctl --failed
```

### Units

```bash
# To see the units
sudo systemctl list-units
```

There are 12 types of units, the services are the most important ones. The `target`s consist of some services and by loading them all of its services are going to run.

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

### Journalctl

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

---

# 101 - 3

## runlevels

You can run Linux on different levels. Runlevels define what tasks can be accomplished in the current state of a Linux System.

### systemd

```bash
# Each target means something
sudo systemctl list-unit --type=target

# More detail for targets
systemctl cat multi-user.target
```

1. `rescue`
    - Local systems are mounted
    - There is no networking
    - Only root user
2. `emergency`
    - Only root file system in read-only mode
    - No networking
    - Only root user
3. `reboot`
4. `halt`
    - Stop all processes
    - Halts CPU activities
5. `poweroff`

```bash
# Change the level
systemctl isolate rescue
systemctl isolate graphical

# Check system running
systemctl is-system-running
```

### SysV runlevels

Different stages for RedHat-based systems.

- 0 - Shutdown
- 1 - Single-User
- 2 - Multi-User
- 3 - Multi-User with networking
- 4 - Customized by the admin
- 5 - Multi-User with networking and graphics
- 6 - Reboot

Different stages for Debian systems.

- 0 - Shutdown
- 1 - Single-User
- 2 - Multi-User with graphics
- 6 - Reboot

```bash
# You can use init to change the mode.
# In our systems, we have init command and
# it is connected to systemd.
# Reboot
init 6

# See the history of the change mode
runlevel
```

`/etc/rc` files like `/etc/rc6.d/` contain some bash files (Linked to the real bash script files) and represent the init commands tasks.

`K` at the beginning of these files means to kill, and `S` means to start.


## Shutdown like a professional

`halt`, `reboot`, `poweroff` are some commands using systemd underlying.

```bash
# Reboot for 5 minutes later with
# the message "we are going down"
shutdown -r +5 we are going down

# Cancelation
shutdown -c

# Wall to send a message
wall hi everybody!
# Disable messagin
mesg n
# What users are logged in the system
# With their messaging status
who -T
```

`/etc/motd` is the message of the day. After ssh to the system, this message is going to be printed.

## Advanced Configuration and Power Interface

ACPI provides an open standard for the operating system to discover and configure computer hardware components. The bold part of this tool is power management.

---
- [Linux1st](https://linux1st.com/)