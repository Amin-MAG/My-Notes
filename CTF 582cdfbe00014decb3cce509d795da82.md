# CTF

## Hydra

It tries to broad-force a bunch of passwords based on the connection. It could be with `ssh` or any other kind of connection.

```bash

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