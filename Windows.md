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

## WinRM

Windows Remote Management, or WinRM, is a Windows-native built-in remote management protocol that basically uses Simple Object Access Protocol to interact with remote computers and servers, as well as Operating Systems and applications.

## NTLM (New Technology Lan Manager)

NTLM is a collection of authentication protocols created by Microsoft. It is a challenge-response authentication protocol used to authenticate a client to a resource on an Active Directory domain.

1. The client sends the user name and domain name to the server.
2. The server generates a random character string, referred to as the challenge.
3. The client encrypts the challenge with the NTLM hash of the user password and sends it back to the server.
4. The server retrieves the user password (or equivalent).
5. The server uses the hash value retrieved from the security account database to encrypt the challenge string. The value is then compared to the value received from the client. If the values match, the client is authenticated.