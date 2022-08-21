# Hashcat

## Basic Usage

Here are the minimum argument and option for running an example.

- Hash type

- Attack mode

- The cypher or hashes

- The wordlists and datasources

This is a basic command of hashcat tool.

```bash
hashcat -m 1000 -a 0 <HASH> ./wordlists/rockyou.txt
```

### Hash Type

You need to specify the algorithm and the type of hash with `-m` or `--hash-type`.

```bash
# For example 1000 for windows nt hashes
hashcat -m 1000
```

### Attack Mode

This tells the hashcat how to find the password. (e.g. dictionary attack)

```bash
# For dictionary attack we use -a 0
hastcat -m 1000 -a 0
```

### Hashes

It's the hash or hash file that you want to crack.

```bash
# A single hash
hastcat -m 1000 -a 0 <HASH>

# A file containing many hashes
hastcat -m 1000 -a 0 <FILENAME>
```

### Wordlist, mask, or directory

The `rockyou.txt` is a dictionary wordlist.

```bash
# The complete command looks like
hashcat -m 1000 -a 0 <HASH> ./wordlists/rockyou.txt
```

## Dictionary Attack

You use a wordlist to hash and find the cypher.

```bash
hashcat -m 1000 -a 0 <HASH> ./wordlists/rockyou.txt
```

### Rules

The default rule is to match passwords if and only if they are the same. We can also add some rules to consider much more. (e.g. letter capitalization, or adding a number to the end of the word)

You can use `-r` switch to specify the rule.

```bash
hashcat -m 1000 -a 0 -r /usr/share/hashcat/rules/best64.rule <HASH> ./wordlists/rockyou.txt
```

## Bruteforce Attack

- `?d` - Digit

- `?l` - Lowercase Letter

- `?u` - Uppercase Letter

- `?s` - Special Character

- `?a` - All

Here is a basic command for bruteforce.

```bash
hashcat -m 1000 -a 3 <HASH> ?a?a?a
```

You can do this incrementally using `-i` switch.

```bash
hashcat -m 1000 -a 3 <HASH> -i ?a?a?a?a?a
```

## Combinator Attack

It will check all of combination of given words. (They're in 2 separate files.)

```bash
hashcat -m 1000 -a 1 <HASH> ./wordlist/english1.txt ./wordlist/english2.txt
```

## PRINCE Attack

```bash
pp64.app wordlist/rockyou.txt | pp64.app | hashcat -O -a 0 -m 0 --status-time 600 hashes.txt
```

> You can use `--status-time` for setting the interval of printing the statistic info.

> You can use `-O` to optimize kernel.

# Resources

- [Introduction to Hashcat - YouTube](https://www.youtube.com/watch?v=EfqJCKWtGiU)
- [Introduction to Hashcat - Part II - YouTube](https://www.youtube.com/watch?v=FZ9g6Pau8ao)
