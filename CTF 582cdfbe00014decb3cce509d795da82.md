# CTF

- `enum4linux -a`
- Linpease for find system vulenterise
- johntheripper can crack the key passphrases
- 

## Gobuster

```bash
gobuster -w <WORDS> -u <IP>
```

## NMap

```bash
nmap -sC -sV -oN nmap/initial <IP>
```

## Hydra

It tries to broad-force a bunch of passwords based on the connection. It could be with `ssh` or any other kind of connection.

```bash
hydra -l <USERNAME> -P <PASSWORD_LIST> ssh://<IP>
```

## Hashcat

It actually try to calculate bunch of string with different hashing algorithms and find the right one to crack test authentication.

```bash

```

## Radare2

To open the Radare2 console

```bash
r2 -w -A -d <BINARY_FILES>
```

### Some basic commands

```bash
# To get all of the functions
afl

# Seek to the main
s main

# Show the code 
vv

# Schematic
VV

```

## GDB