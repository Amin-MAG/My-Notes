# Linux

## Directories

- `/bin`: Essential executables, It contains some binary files that are essential for the system.
- `/sbin`: Essential executables for root.
- `/lib`: Shared code between binaries, `/bin` binaries, and `/sbin` binaries may share some codes in it.
- `/usr/bin`: Non-Essential installed binaries.
- `/usr/local/bin`: Locally compiled binaries.
- `/etc`: Editable text config, all of the configurations are here.
	-  `/etc/skel/`:  it contains the skeleton of home directories.
	- `/etc/passwd`: it contains all the details for users.
	- `/etc/shadow`: it contains all passwords for users.
	- `/etc/gshadow`: it contains all passwords for groups.
- `/home`: User data
- `/boot`: Files need to boot the system like the kernel itself.
- `/dev`: Device files
- `/opt`: Optional and add-on software.
- `/var`: Contains variable files that will be changed as the OS is being used. Like logs and cache files.
- `/tmp`: Contains files that won't be persisted between reboots.
- `/proc`: Imaginary directory that doesn't actually exist on the disk. It is created by memory and contains information about processes.

# Logs

## Ubuntu

- `/var/log/auth.log`: contains the login attempts.
- `/var/log/syslog`: contains logs related to the hardware and system events.
- `/var/log/apt/`: contains the logs related to the package manager. `history.log` contains the previous commands and installations.
- `/var/log/dmesg` or `dmesg`: shows the same information in `syslog` in a special format.
- `journalctl`: inspects actual units in `systemd`.

## Fedora

- `/var/log/boot.log`: contains the log of the boot process.
- `/var/log/dnf`: contains the log of package manager.

# See More

- [LPIC-1](Linux/LPIC-1.md)
- [Swap](Linux/Swap.md)
- [Linux Commands](Linux/Linux%20Commands.md)
- [DPKG](Linux/DPKG.md)
- [Makefile](Linux/Makefile.md)
- [Systemctl](Linux/Systemctl.md)
- [ffmpeg](Linux/ffmpeg.md)