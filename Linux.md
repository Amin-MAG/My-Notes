# Linux

## Directories

- `/bin`: Essential executables, It contains some binary files that is essential for system.
- `/sbin`: Essential executables for root.
- `/lib`: Shared code between binaries, `/bin` binaries and `/sbin` binaries may share some codes in it.
- `/usr/bin`: Non-Essential installed binaries.
- `/usr/local/bin`: Locally compiled binaries.
- `/etc`: Editable text config, all of the configurations are here.
- `/home`: User data
- `/boot`: Files need to boot the system like the kernel itself.
- `/dev`: Device files
- `/opt`: Optional and add-on software.
- `/var`: Contains variable files that will be changed as the OS is being used. Like logs and cache files.
- `/tmp`: Contains files that won't be persisted between reboots.
- `/proc`: Imaginary directory that doesn't actually exist on the disk. It is created by the memory and contains information about processes.

[LPIC-1](Linux/LPIC-1.md)

[Swap](Linux/Swap.md)

[Linux Commands](Linux/Linux%20Commands.md)

[DPKG](Linux/DPKG.md)

[Makefile](Linux/Makefile.md)

[Systemctl](Linux/Systemctl.md)
