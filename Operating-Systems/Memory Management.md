# Memory Management

Here is a figure for the memory of the OS

![Untitled](Operating-Systems/Memory%20Management/Untitled.png)

1. The text area contains the program's machine instructions (i.e., the executable code).
2. [Global](https://icarus.cs.weber.edu/~dab/cs1410/textbook/1.Basics/terminology.html#extern) variables are defined in
   global scope outside of any function or
   object; [static variables](https://icarus.cs.weber.edu/~dab/cs1410/textbook/1.Basics/data.html#auto_static) have the
   keyword `static` included as part of their definition. The memory that holds global and static variables is typically
   allocated at program startup.
3. The space illustrated here was allocated but unused on a segmented-memory system and provided space where the stack
   and heap could grow. This space is not allocated on a paged memory system but signifies space available for stack and
   heap growth.
4. The stack (sometimes called the runtime stack) contains all of
   the [automatic](https://icarus.cs.weber.edu/~dab/cs1410/textbook/1.Basics/data.html#auto_static) (i.e., non-static)
   variables.
5. Memory is allocated from and returned to the heap with with the `new` and `delete` operators respectively.

## Stack

A stack is a simple last-in, first-out (LIFO) data structure. Imagining a stack of plates in a cafeteria is common to
introduce stacks to new computer scientists. In the cafeteria example, plates are removed from or added to the stack
only at the top. One of our first actions upon entering the cafeteria is to *pop* a plate off the top of the stack of
plates before we go through the line. Similarly, the dishwashers *push* each clean plate on top of the stack one at a
time. In this way, the last clean plate pushed on the stack is the first plate that a customer pops off of the top.
Inserting or removing a plate from the middle of the stack is not permitted. Stacks must support at least two
operations: push and pop; other operations are possible but are not required.

Calling another function from within a function pushes a new **[frame](https://en.wikipedia.org/wiki/Call_stack)** onto
the stack, which will contain the values of that function and so on. When the called function returns, its stack frame
is popped off the stack. You might be familiar with the stack from when you’re debugging a crashed program. Most
language compilers will return a stack trace to aid in debugging, which displays the functions that have been called
leading up to that point.

## Heap

In contrast, the heap is more flexible than the stack. Whereas the stack only allows allocation and deallocation at the
top, programs can allocate or deallocate memory anywhere in a heap. So, the program must return memory to the stack in
the opposite order of its allocation. But the program can return memory to the heap in *any* order. That means it's
possible to have a "hole" in the middle of the stack - unallocated memory surrounded by allocated memory.

There is only one restriction on the memory allocated to the program from the heap: it must form
a [contiguous](http://www.thefreedictionary.com/contiguous) block large enough to satisfy the request with a single
chunk of memory. This one restriction increases the complexity of a heap in at least two ways: First, the code that
carries out the allocation operation must scan the heap until it finds a contiguous block of memory that is large enough
to satisfy the request. Second, when memory is returned to the stack, adjacent freed blocks must
be [coalesced](http://www.thefreedictionary.com/coalesced) to better accommodate future requests for large blocks of
memory. The heap's increased complexity means that managing memory with a heap is slower than with a stack. But a heap
also has advantages that justify the increased overhead.

The heap is **an area of dynamically-allocated memory that is managed automatically by the operating system** or the
memory manager library. Memory on the heap is allocated, deallocated, and resized regularly during program execution,
and this can lead to a problem called fragmentation.

A very simple explanation is that the **heap** is the portion of memory where *dynamically allocated* memory resides (
i.e. memory allocated via `malloc`). Memory allocated from the heap will remain allocated until one of the following
occurs:

1. The memory is `free`'d
2. The program terminates

If all references to allocated memory are lost (e.g. you don't store a pointer to it anymore), you have what is called
a *memory leak*. This is where the memory has still been allocated, but you have no easy way of accessing it anymore.
Leaked memory cannot be reclaimed for future memory allocations, but when the program ends the memory will be free'd up
by the operating system.

## Memory management in code

As programs run they write objects to memory. At some point these objects should be removed when they’re not needed
anymore. This process is called **memory management**.

In a language like C the programmer will call a function such as `malloc` or `calloc` to write an object to memory.
These functions return a pointer to the location of that object in heap memory. When this object is not needed anymore,
the programmer calls the `free` function to use this chunk of memory again. This method of memory management is
called **explicit deallocation** and is quite powerful. It gives the programmer greater control over the memory in use,
which allows for some types of easier optimisation, particularly in low memory environments. However, it leads to two
types of programming errors.

1. One, calling `free` prematurely which creates a **dangling pointer**. Dangling pointers are pointers that no longer
   point to valid objects in memory.
2. Two, failing to free memory at all. If the programmer forgets to free an object they may face a **memory leak** as
   memory fills up with more and more objects.

## Garbage Collector

It is **automatic dynamic memory management.**

- increased security
- better portability across operating systems
- less code to write
- runtime verification of code
- bounds checking of arrays

A running program will store objects in two memory locations,
the **[heap](https://en.wikipedia.org/wiki/Memory_management#HEAP)** and
the **[stack](https://en.wikipedia.org/wiki/Stack-based_memory_allocation)**. Garbage collection operates on the heap,
not the stack.

# Read More

- [Golang Memory Management](./../Golang/Go%20Memory%20Management.md)

# Resources

[4.6. Memory Management: Stack And Heap](https://icarus.cs.weber.edu/~dab/cs1410/textbook/4.Pointers/memory.html)