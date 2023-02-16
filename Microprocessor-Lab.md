# Microprocessor Lab

## Basics

### Pin

The bits used for the input.

```c
// To make port A input 
DDRA = 0x00;
```


### Port

The bits used for the output.

```c
// To make port B output
DDRB = 0xff;
```

### Delay

To use delay feature:

```c
#include <delay.h>

delay_ms(30);
```

## LCD

You need to initilize your LCD first, connect it to port C.

```c
lcd_init(16);
lcd_clear();
```

To write on the LCD:

```c
// Single character
lcd_putchar('R');
// A string
lcd_puts("Amin\nMAG")
```

To change the cursor

```c
lcd_gotoxy(1,0);
```

## Seven-Segment

First of all, it's better to define your seven-segment data in an array.

```c
char code[] = { 0x3F, 0x06, 0x5B, 0x4F,0x66,0x6D,0x7D,0x07,0x7F,0x6F, 0x77,0x7C, 0x39 , 0x5E, 0x79, 0x71 };
```

## Terminal

You need turn on the receiver and transmiter when you create your project.

```c
// To get a character from the terminal
char a = getchar();

// To print a character on terminal
putchar();
```

> **Note:** I always make the terminal port input. (DDRD = 0x00)

To go to the next line you need to putchar values `\r` and `\n`.

## Timer 

You can set a timer to give you interrupts. This will geenrate a function in your code.

```c
// Timer 0 output compare interrupt service routine
interrupt [TIM0_COMP] void timer0_comp_isr(void)
{
	// Place your code here
	updateSeven();
}
```
