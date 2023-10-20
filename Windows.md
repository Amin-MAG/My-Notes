# Windows

## SMB (Service Message Block)

The Service Message Block (SMB) is a network protocol used by Microsoft Windows and other operating systems for sharing files, printers, and various resources on a local area network (LAN) or across the internet. It works on port 445. You might see `microsoft-ds` when you are scanning this port.

```bash
# -L This option allows you to look at what services are available on a server.
smbclient -L 192.168.1.1

# To connect to the SMB
smbclient \\\\192.168.1.1\\share_name
```

To list the files

```bash
smb: \> ls
```

To change directory

```bash
smb: \> cd test
```

To download a file

```bash
smb: \> get flag.txt
```
