# DPKG

# What is DPKG?

In a nutshell, DPKG is a low-level system tool to extract, analyze, unpack, install or remove `.deb` files.

# Debian files

A deb file is an archive that contains data. It is used to easily distribute and install programs for Linux Debian and derivatives.

## Anatomy of a deb package

```bash
<name>_<version>-<revision>_<architecture>.deb
```

That is:

- `<name>` – the name of your application;
- `<version>` – the version number of your application;
- `<revision>` – the version number of the current deb package;
- `<architecture>` – the hardware architecture your program will be run on.

For example, suppose you want to release your program called hello, version 1.0, built for 64-bit ARM processors. Your deb file name would look something like `hello_1.0-1_arm64.deb`.

## Making the Debian package

### 1. Create the package directory

Create a directory. This name should be in a Debian-Package format.

```bash
mkdir hello_1.0-1_arm64
```

### 2. Create the structure

The folder actually represents the root path of the system using the Debian package. So now you should create folders like `/usr/local/bin/` and then copy your executable binary files in them.

```bash
mkdir -p hello_1.0-1_arm64/usr/local/bin
cp ~/YourProjects/Hello/hello hello_1.0-1_arm64/usr/local/bin
```

### 3. Create the control file

The control file lives inside the `DEBIAN` directory. Mind the uppercase: a similar directory named `debian` (lowercase) is used to store source code for the so-called source packages.

```bash
mkdir helloworld_1.0-1_arm64/DEBIAN
touch helloworld_1.0-1_arm64/DEBIAN/control
```

### 4. Fill in the control file

Open the file previously created with your text editor of choice. The `control` file is just a list of data fields. For binary packages there is a minimum set of mandatory ones:

- `Package` – the name of your program;
- `Version` – the version of your program;
- `Architecture` – the target architecture;
- `Maintainer` – the name and the email address of the person in charge of the package maintenance;
- `Description` – a brief description of the program.

For example:

```
Package: hello
Version: 1.0
Architecture: arm64
Maintainer: Internal Pointers <info@internalpointers.com>
Description: A program that greets you.
 You can add a longer description here. Mind the space at the beginning of this paragraph.
```

The `control` file may contain additional useful fields such as the `[section](https://www.debian.org/doc/debian-policy/ch-archive.html#s-subsections)` it belongs to or the [dependency list](https://www.debian.org/doc/debian-policy/ch-relationships.html#s-binarydeps). The latter is extremely important in case your program relies on external libraries to work correctly. You can fill it manually if you wish, but there are helper tools to ease the burden. I will show you how in the next few paragraphs.

### 5. Build the Debian package

This is the last step for creating a Debian package.

This is the last step. Invoke `dpkg-deb` as following:

```bash
dpkg-deb --build --root-owner-group <package-dir>
```

So in our example:

```bash
dpkg-deb --build --root-owner-group helloworld_1.0-1_arm64
```

The `--root-owner-group` flag makes all deb package content owned by the root user, which is the standard way to go. Without such a flag, all files and folders would be owned by your user, which might not exist in the system the deb package would be installed to.

The command above will generate a nice `.deb` file alongside the working directory or print an error if something is wrong or missing inside the package. If the operation is successful you have a deb package ready for distribution. Keep reading for additional goodies!

Make sure you have the `dpkg-deb` program installed in your system.

## Test and Instal the output Debian file

It's a good idea to test your deb package once created. You can install it like any other regular deb package:

```bash
sudo dpkg -i <APP_NAME>
```

Make sure it can be also uninstalled easily. You can just remove the package:

```bash
sudo dpkg -r <APP_NAME>
```

or remove it along with the configuration files (if any):

```bash
sudo dpkg -P <APP_NAME>
```

Make sure the application has been removed correctly by issuing:

```bash
dpkg -l | grep <APP_NAME>
```

The `dpkg -l` command lists all the packages installed, while `grep` searches for `<appname>`. The output should be blank if the app has been uninstalled correctly.

## **Run scripts before or after package installation and removal**

Four files: `postinst`, `preinst`, `postrm`, and `prerm` are called *maintainer scripts*. Such files live inside the `DEBIAN` directory and, as their names suggest, `preinst` and `postinst` are run before and after installation, while `prerm` and `postrm` are run before and after removal. They must be marked as executables. Also, remember to set permissions: must be between `0555` and `0775`.

# Resource

[Building binary deb packages: a practical guide](https://www.internalpointers.com/post/build-binary-deb-package-practical-guide)

[Debian Policy Manual](https://www.debian.org/doc/debian-policy/index.html)

[Chapter 7. Basics of the Debian package management system](https://www.debian.org/doc/manuals/debian-faq/pkg-basics.en.html)

[How to uninstall a .deb installed with dpkg?](https://unix.stackexchange.com/questions/195794/how-to-uninstall-a-deb-installed-with-dpkg)