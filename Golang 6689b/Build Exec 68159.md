# Build Executive Binaries

# Go Get

The `go get` tool can fetch packages from version control systems like GitHub. Under the hood, go get clones packages into subdirectories of the `$GOPATH/src/` directory. Then, if applicable, it installs the package by building its executable and placing it in the `$GOPATH/bin` directory. If you configured Go as described in the prerequisite tutorials, the `$GOPATH/bin` directory is included in your `$PATH` environmental variable, which ensures that you can use installed packages from anywhere on your system.

Use `go get` to fetch and install Caddy:

```bash
go get -u github.com/mholt/caddy/caddy
```

The command will take some time to complete, but you won’t see any progress while it fetches the package and installs it. No output actually indicates that the command was executed successfully.

# Go Build

Execute `go build` and specify the package import path:

```bash
go build github.com/mholt/caddy/caddy
```

As before, no output indicates successful operation. The executable will be generated in your current directory, with the same name as the directory containing the package. In this case, the executable will be named `caddy`.

If you’re located in the package directory, you can omit the path to the package and simply run `go build`.

To specify a different name or location for the executable, use the `-o` flag. Let’s build an executable called `caddy-server` and place it in a `build` directory within the current working directory:

```bash
go build -o build/caddy-server github.com/mholt/caddy/caddy
```

This command creates the executable and also creates the `./build` directory if it doesn’t exist.

# Go Install

Building an executable creates the executable in the current directory or the directory of your choice. Installing an executable is the process of creating an executable and storing it in `$GOPATH/bin`. The `go install` command works just like `go build`, but `go install` takes care of placing the output file in the right place for you.

To install an executable, use `go install`, followed by the package import path. Once again, use Caddy to try this out:

```bash
go install github.com/mholt/caddy/caddy
```

Just like with `go build`, you’ll see no output if the command was successful. And like before, the executable is created with the same name as the directory containing the package. But this time, the executable is stored in `$GOPATH/bin`. If `$GOPATH/bin` is part of your `$PATH` environmental variable, the executable will be available from anywhere on your operating system.

# Building Executables for Different Architectures

The `go build` command lets you build an executable file for any Go-supported target platform, on your platform. This means you can test, release and distribute your application without building those executables on the target platforms you wish to use.

Cross-compiling works by setting required environment variables that specify the target operating system and architecture. We use the variable `GOOS` for the target operating system and `GOARCH` for the target architecture. To build an executable, the command would take this form:

```bash
$ env GOOS=target-OS GOARCH=target-architecture go build package-import-path
```

The `env` command runs a program in a modified environment. This lets you use environment variables for the current command execution only. The variables are unset or reset after the command executes.

All of the combinations. `GOOS`, `GOARCH`

```bash
- android, arm
- darwin, 386
- darwin, amd64
- darwin, arm
- darwin, arm64
- freebsd, 386
- freebsd, amd64
- freebsd, arm
- linux, 386
- linux, amd64
- linux, arm
- linux, arm64
- linux, ppc64
- linux, ppc64le
- linux, mips
- linux, mipsle
- linux, mips64
- linux, mips64le
- netbsd, 386
- netbsd, amd64
- netbsd, arm
- openbsd, 386
- openbsd, amd64
- openbsd, arm
- plan9, 386
- plan9, amd64
- solaris, amd64
- windows, 386
- windows, amd64
- dragonfly, amd64
```

**Warning:** Cross-compiling executables for Android requires the [Android NDK](https://developer.android.com/ndk/index.html) and some additional setup which is beyond the scope of this tutorial.

Using the values in the table, we can build Caddy for Windows 64-bit like this:

```bash
env GOOS=windows GOARCH=amd64 go build github.com/mholt/caddy/caddy
```

Once again, no output indicates that the operation was successful. The executable will be created in the current directory, using the package name as its name.

# Resource

[How To Build Go Executables for Multiple Platforms on Ubuntu 16.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-build-go-executables-for-multiple-platforms-on-ubuntu-16-04)