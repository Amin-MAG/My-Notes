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

# 102 - 1

## Unix Directories

Understanding Filesystem Hierarchy Standard or FHS can help you find your programs, configs, logs, or other files.

- `/bin`: Essential executables, It contains some binary files that are essential for the system.
- `/sbin`: Essential executables for root.
- `/lib`: Shared code between binaries, `/bin` binaries, and `/sbin` binaries may share some codes.
- `/usr`: Second Hierarchy
  - `/usr/bin`: Non-Essential installed binaries.
  - `/usr/local/bin`: Locally compiled binaries.
- `/etc`: Editable text config, all of the configurations are here.
- `/home`: User data
- `/mnt`: Mount point for mounting a filesystem temporarily
- `/boot`: Files need to boot the system like the kernel.
- `/dev`: Device files
- `/opt`: Optional and add-on software.
- `/var`: Contains variable files that will be changed as the OS is used. Like logs and cache files.
- `/tmp`: Contains files that won't be persisted between reboots.
- `/proc`: Imaginary directory that doesn't exist on the disk. It is created by the memory and contains information about processes.
- `/root`: Home of the root user (For security)

## Partitions

Before formatting, you need to partition your hard disk.

In the Linux world, the devices are defined as `/dev/`. Your flash drive will be mounted at `/mnt/`. So everything becomes part of the `/` tree. 

You can create different partitions for different directories of the root; for example, you can create:  

- 250MB partition for your `/boot/`.

- 905GB partition for your `/home/`.

- 50GB partition for your `/` (The rest of the directories) - In this case, when you want to change your Operating system, you need to install it on this partition. You can easily mount your old partition `/home/` and not be worry about your files.

> In BIOS you could have upto 4 partitions on each disk. (Although you can create a extended partition, then create another 4 logical partition on that.)

### Tools

#### Fdisk command

```bash
# Run fdisk for sda disk
sudo dfisk /dev/sda
# Command (m for help):
# p -> shows the partitions
# q -> exit
```

#### Mount

```bash
sudo mount | grep sda 
```

#### Parted

```bash
sudo parted /eve/sda p
# You can use the graphical version Gparted
```

### LVM

What about having a 20TB hard drive? The answer is LVM or Logical Volume Managment. You can connect ten 2TB hard disk and use LVM to create a single virtual hard drive. After that you can start to partition this 20TB virtual LVM hard drive.

You can easily connect more hard drives and make each one of your partitions bigger.

The main concepts:

- Physical Volume (or PV)

- Volume Group (or VG)

- Logical Volume (or LG)

```bash
# It is not for LPIC 1
# You can use the lv command
lv
```

## Design Hard disk layout

### swap

The swap is like an extended memory. Kernel will page memory and then it can execute a program that need more memory than the system has. (Although it is slow)

### boot

Having separate partition for `/boot` ?

1. You can recover your system using `/boot` partition.

2. You can not encrypt `/boot` or load it from network because the UEFI or BIOS needs to read the data from this partition, but by separating `/boot/` and `/` partitions you can encrypt just `/` partition, or you even can put the `/` in another disk drive. (It is much cleaner.)

---

# 102 - 2

## Grub 1

Common Configurations

```bash
# The forground and background of items
color= 

# The default OS
default=

# If it fails...
fallback=

# Graphical options
hiddenmenu=
splashimage=

# Timeout for default
timeout=

# Authentication
password=

# Default OS is the previous loaded OS
savedefault=
```

For each one of operating systems, You need to configure these items.

```bash
# The name of the OS.
title=
# Where the /boot directory is.
root=
kernel=
initrd=
rootverity=
chainloader=
```

You need to apply these changes.

```bash
grub-install /dev/sda1
```

## Grub 2

It is more like a programming language.

```bash
set default ="0"
menuentry "Fedora" {
	# Wher the /boot is
	set root=<DISK>
	# Linux Kernel
	linux <KERNEL>
	# Initramfs
	initrd <INITRD>
}
menuentry "Windows" {
	chainloader
}
```

To aggregate seperate config files and update the new configuration file.

```bash
grub2-mkeconfig > /boot/grub2/grub.cfg
```

### Grub CLI

```bash
# Print everything even the disks
ls
ls (hd0.gtp3)/

# To load the kernel with CLI
linux <PATH-TO-KERNEL> ro root=<BOOT-DIR>
initrd <PATH-TO-INITRD-IMAGE>

# Start to boot the kernel
boot
```

---

# 102 - 3

## Linking 

When you create your program, you use others codes known as a library. The process of using these libraries is called linking.

### Static Linking

It's when you add the library to your executable program.

- It is easy to run. We are sure that it will be executed.
- It needs lots of disk storage.
- When the library is updated, your executable program won't be updated.

### Dynamic Linking

It tells the operating system that it needs this library. Linux dynamic libraries have names like `lib<LIB_NAME>.so.<VERION>` located at places like `/lib/`, `/lib64/` or `/usr/lib/`.

- It needs a small amount of disk storage.
- It's more secure because your program will also be updated when the library is updated.

To check the dependencies

```bash
# Shows the libraries you need to run the program
ldd /usr/bin/ls
```

The configuration specifies where to find the libraries.

```bash
cat  /etc/ld.so.conf
```

There is a cache file that has a better structure for searching.

```bash
# To recreate the cache file
sudo ldconfig -v

# Show the cache file
sudo ldconfig -p
```

To analyze an ELF file

```bash
# To link the libraries manually
/usr/lib64/ld-linux-x86-64.so.2 /usr/bin/ls

readelf -Wl /usr/bin/ls
```

#### Steps of finding libraries

1. `LD_LIBRARY_PATH` environment variable
2. Programs `PATH`
3. `/etc/ld.so.conf`
4. `/lib/`, `/lib64/`, `/usr/lib/`, `/usr/lib64/`

---

# 102 - 4

## Debian package management

Package managers are responsible for searching the repositories, installing packages and their dependencies, resolving conflicts and updating system.

In Debian-base distros, there are some package manager softwares like

- apt-get
- dpkg (lower level)
- apt (easy to use)

Debian packages are in the format of `<APP_NAME>-<VERSION>-<ARCH>.deb`. You can read more [here](DPKG.md).

### apt

You can find the apt sources in `/etc/apt/sources.list` or `/etc/apt/source.list.d`. 

```bash
# deb <SOURCE_PATH> <VERSINO_TYPE> <RESOURCES...>
# SOURCE_PATH can be URL or cdrom or ....
deb http://ir.archive.ubuntu.com/ubuntu/ focal-updates main restricted
```

The `source.list.d` is a directory contains list of `.list` source files.

#### Update package's lists

It looks at the `.list` source files and update the list of the packages.

```bash
sudo apt-get update

# After updating the list of packages 
# you're able to install a package.
sudo apt-get install vlc
```

These caches are in `/var/cache/apt`.


#### Install/Uninstall a package

Here are some basic commands.

```bash
sudo apt-get install <APP_NAME>

# To simulate the installation
sudo apt-get install -s <APP_NAME>

# To download the packages and save them in cache
sudo apt-get install --download-only <APP_NAME>

# To install dependencies haven't been installed
sudo apt-get install -f

# To download the .deb files
apt-get download <APP_NAME>

# To uninstall a package
sudo apt-get remove <APP_NAME>

# Remove with the configurations
sudo apt-get purge <APP_NAME>

# To remove unused packages (dependencies)
sudo apt-get autoremove
```

#### Search packages

```bash
# apt-get
apt-cache search vlc
# apt
apt search vlc
```

#### Upgrade

It compares the installed packages with local repository and install new packages if there is a newer version in local repository.

```bash
sudo apt update
sudo apt upgrade

# Upgrade the whole OS
sudo apt-get dist-upgrade
```

### dpkg

It's a tool for installing a `.deb` file.

```bash
# Install the .deb file
sudo dpkg -i <APP_NAME>.deb

# Get the list of installed packages
sudo dpkg -l

# Get information
sudo dpkg -I <APP_NAME>.deb

# Remove a package
sudo dpkg -r <APP_NAME>

# Remove with the configurations
sudo dpkg -P <APP_NAME>

# Show which dependencies binary files were installed
sudo dpkg -L <APP_NAME>

# Show which app installed the package.
# It's the opposite of the previous command.
sudo dpkg -S <APP_NAME>
```

### Reconfigure packages

```bash
sudo dpkg-reconfigure <APP_NAME>
```

# 102 - 5

## Redhat package management (RPM, YUM)
All `.rpm` files just like `.deb` files can be installed with RPM commands.

> We also have something called `zypper` for SUSE linux.

## Yum

- Yum is a higher level tool that can install packages for you.  The yum repositories are in `/etc/yum.repos.d/`.
- You can configure the behavior of the Yum package manager with `/etc/yum.conf`.
- Yum doesn't have a debain `update` concept.
- The `dnf` is a newer package manager and it's the Fedora undelying package manager.


### Install/Uninstall a package

```bash
sudo yum -y install vlc
sudo yum localinstall vlc.rpm
sudo yum groupinstall "KDE Plasma Workspaces"

sudo yum -y reinstall vlc

sudo yum remove vlc
```

### Upgrade a package

```bash
sudo yum update
# This one will remove the obsolate packages
sudo yum upgrade
```
 
### List installed packages

```bash
sudo yum list
```

###  Get information about packages

```bash
# Search in repository
sudo yum search vlc

# Get some general information about the package
sudo yum info vlc

# Show dependencies
sudo yum deplist vlc
```

## RPM Files

The `.rpm` files are technically a `cpio` file. You can use `rpm2cpio` to convert to a `cpio` file.

```bash
# Create the file
rpm2cpio <RPM_FILE> > file.cpio
# Extract
cpio -idv < file.cpio
```

Anyway, here are some basic RPM commands.

```bash
# To install or upgrade
# -v is for verbose
# -h for progress bar
sudo rpm -Uvh <RPM_FILE>

# To install
sudo rpm -iv <RPM_FILE>

# To erase
sudo rpm -ev <RPM_FILE>

# To see the signitures
sudo rpm -kv <RPM_FILE>

# To verify the installed package
sudo rpm -Vv <RPM_FILE>
```

### Query

```bash
# To query
sudo rpm -q <RPM_FILE>
# Config files
sudo rpm -qc <RPM_FILE>
# All Installed files
sudo rpm -ql <RPM_FILE>
# Package information
sudo rpm -qc <RPM_FILE>
# Installed packages
sudo rpm -qa 
# Which packages provides this binary
sudo rpm -q --whatprovides <RPM_FILE>
# Show dependencies
sudo rpm -qR <RPM_FILE>
```


# 102 - 6

## Virtualzation

We need Hypervisor is to run a virtual machine.

> To see the CPU information you can use `lscpu` or `cat /proc/cpuinfo`.

### Hypervisors

There 2 types of hypervisors.
- Native or bare metael  Hypervisors
- Hosted Hypervisors

#### Native Hypervisor

The hypervisor directly runs on the hardware and has different kinds of operating systems. Like kvm, Xen, Hyper-V.

#### Hosted Hypervisors

The operating system is installed on the hardware and on the top of that there is a hypervisor with different kinds of operating systems. Like virtualbox, vmware.

### Open Virtualization Format

It's easy to move machines between hypervisors using Open Virtualization Format or OVF. It may include several files. Open Virtualization Archive or OVA is a single file that container all previous standard OVF files.

### Cloning Machine

When you clone a machine you should take care of some network stuff before booting. Here are some examples:

- Host Name 
- MAC Address
- Network Interface Controller
- Machine ID
- Encryption Keys
- HDD UUID
- ...
 
### Containers

This is an OS-Level virtualization. Containes against virtual machines only install the libraries and binaries to run an application. There is no operaing system. Here we need a container engine like Docker to create containers.

# 103 - 1

There are different kinds of shells for example, `bash` (the most common one), `dash`, `zsh`, `ksh`, and so on.

```bash
# To see the name of shell
ls -ltrh /bin/bash
echo $SHELL
```

To see if a command is an internal command or external, you can use `type` command.

```bash
type cd

# Get more info about commands
which vi
whereis vi
whatis vi
```

To ignore special characters like `*` or `$` you can use `\`.

> You can find most of this section in [Linux Commands](Linux%20Commands.md)
> 

# 103 - 2

## Work with streams

- stdin
- stdout
- stderr

> You can find most of this section in [Linux Commands](Linux%20Commands.md)
> 

### Hashes

- md5sum
- sha256sum
- sha512sum

# 103 - 3

## Perform basic file management

### Wildcards

- * - match everything 
- ? - match any single character
- [ABC] or [A-Z] - match bunch of characters
- [!X] - match not having a character

```bash
# copy all of the files
cp ./* /tmp

# list all txt files
ls ./*.txt
```

> ⚠️ You can find most of this section in [Linux Commands](Linux%20Commands.md)
> 


> ✍️  `/dev/zero` is a device that always return zero.
> 

### Compression

- gzip
- bzip2
- tar
- cpio

> ⚠️ You can find most of this section in [Linux Commands](Linux%20Commands.md)
> 

# 103 - 4

## Use streams, pipes and redirects

### Streams

Linux has three kinds of stream: stdin(code=0), stdout(code=1), and stderr(code=2) which can be read from or write on serveral devices such as console, printer, or...

| Operator    | Usage                           |
| ----------- | ------------------------------- |
| `>`           | Redirects STDOUT to a file|
| `>>`          | Appends STDOUT to a file|
| `2>`   | Redirects STDERR to a file|
| `2>>`   | Appends STDERR to a file|
| `&>`   | Appends STDOUT & STDERR to a file|
| `&>>`   | Appends STDOUT & STDERR to a file|
| `<`   | Redirects STDIN from a file|
| `<>`   | Redirects STDIN from a file and send the STDOUT to it|


```bash
# To redirect stdout and stdin in separate files
# Consider not having x* file in current directory
# And having t* file in current directory
ls x* t* > lsresult 2> lserr
```

> ✍️  `&0`, `&1`, `&2` is defined after redirecting and it refers to the destination.
> 

```bash
ls > lsresult 2>&1
```

> ✍️  `/dev/null` ignores any streams that are redirected to it.
> 

```bash
# To just ignore the output
# To redirect stdout and stdin in separate files
# Consider not having x* file in current directory
# And having t* file in current directory
ls x* t* > lsresult 2>/dev/null
```

#### Here documents `<<`

You can use `<< EOF` to give multiple line string to a command. You can use anything instead of using `EOF`.

```bash
cat << EOF > save.txt
```


# Resources

- [Linux1st](https://linux1st.com/)