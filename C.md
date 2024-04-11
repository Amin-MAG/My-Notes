# C

Hello World in C :))

```c
#include <stdio.h>

int main(){
    printf("Hello World!!\n");
}
```

To Compile the code with the assembly files

```bash
gcc app.c --save-temp
```

To tell your compiler to optimize your code

```bash
gcc -O app.c
```

### Volatile

```c
// Tells the compiler not to optimize this variable
voletile _Bool detected = 0;
```

# Using Libraries and Dependencies

Compile a code using the math library

```bash
gcc -o run main.c -lmath
```

Compile a code using the libraries `hb` and `hb-ft`

```bash
# For linux
gcc -o main main.c -lharfbuzz -I/usr/include/harfbuzz -I/usr/include/freetype2 -lfreetype
# For mac
gcc -o main main.c -lharfbuzz -I/usr/local/Cellar/harfbuzz/8.4.0/include/harfbuzz -I/usr/local/Cellar/freetype/2.13.2/include/freetype2 -lfreetype
```

The `-I` flag and the `-l` flag serve different purposes in the context of compiling and linking C programs.

## `-l` Flag

This flag is used to specify libraries to link against during the linking phase. The linker then searches for the corresponding library file to resolve any undefined references to functions or symbols from that library.

## `-I` Flag

This flag is used to specify include directories during compilation. The compiler searches these directories for header files when compiling your code.
# See More

- [gdb](gdb.md)