# CTF

- `enum4linux -a`
- Linpease for find system vulenterise
- johntheripper can crack the key passphrases
- scapy
- hexedit

## ZIP Cracking

```bash
fcrackzip -D -p /usr/share/wordlists/rockyou.txt 37366.zip -u
```

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

# `httpry`

# Gdb

# Radare2

To open the Radare2 console

```bash
# To write, analyze in debug mode
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
# Then you can change the mode with p
VV
```

Edit a hex number from operators or operands

```bash
# Write hex at the head
:wx 75
```

To debug continue

```bash
:dc
```

dig

gdb