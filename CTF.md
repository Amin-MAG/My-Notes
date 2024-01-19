# CTF

- `enum4linux -a`
- Linpease for find system vulenterise
- johntheripper can crack the key passphrases
- scapy
- hexedit

## ZIP Cracking

```bash
fcrackzip -D -p /usr/share/wordlists/rockyou.txt 37366.zip -u
# john2zip and john
```

## Gobuster

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

# Reverse

To see all the readable strings in a binary that you can use

```bash
strings ./chall
```

To see the system calls you can use

```bash
strace ./chall test
```

## Ghidra

## Gdb

## Radare2

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

## dig

# Web

- SSTI or SERVER-SIDE TEMPLATE INJECTION
	- [SSTI in Flask/Jinja2](https://medium.com/@nyomanpradipta120/ssti-in-flask-jinja2-20b068fdaeee)
	- [HACKTHEBOX – TEMPLATED WRITE-UP](https://shakuganz.com/2021/06/13/hackthebox-templated-write-up/)
- 