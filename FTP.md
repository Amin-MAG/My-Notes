# FTP

One the best options is `vsftpd` that you can install on your server.

```bash
sudo apt update
sudo apt install vsftpd
```

To enable the service

```bash
sudo service enable vsftpd
```

The configuration file is in the `/etc/vsftpd.conf`. Now, you can use your favorite ftp client like `FileZilla` to connect to this FTP server.

## To connect 

You can transfer your files using FTP

```bash
ftp <IP_ADDRESS>
```

# FTP CLI

To list the files

```bash
ftp> ls
```

To download a file

```bash
ftp> get flag.txt
```
