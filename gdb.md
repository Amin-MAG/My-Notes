# gdb

The GNU Debugger is a portable debugger that runs on many Unix-like systems and works for many programming languages, including Ada, C, C++, Objective-C, Free Pascal, Fortran, Go, and partially others.

## Debug a C program

You can give the c compiled program to gdb and jump into the gdb environment. 

```bash
gdb helloworld.o
```

Here are some useful and basic commands.

```bash
# To run the program
(gdb) run

# To see the stack, you can use
(gdb) info stack

# To see the frames in the stack
(gdb) info frame

# To continue the program
(gdb) continue
```

>  use `(gdb) help` to see all of the commands
