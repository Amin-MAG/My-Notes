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

# See More

- [gdb](gdb.md)