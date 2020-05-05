# Smashing the stack for fun and profit for new age
## https://travisf.net/smashing-the-stack-today

We will be working with 32 bit library

### installing necessary packages
```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 \
    g++-multilib build-essential gdb
```

```
sudo apt-get update
sudo apt-get install build-essential gdb
```

### Compiling the example code : 
`gcc -m32 -fno-stack-protector -z execstack -D_FORTIFY_SOURCE=0 -o example1 example1.c`

```
    -m32: compile for 32-bit
    -fno-stack-protector: disable stack canaries
    -z execstack: ensure the stack is executable (disable NX bit protection)
    -D_FORTIFY_SOURCE=0: disable FORTIFY_SOURCE

```

### to disable ASLR

`sudo sysctl -w kernel.randomize_va_space=0`

### to enable ASLR

`sudo sysctl -w kernel.randomize_va_space=2`

# Notes

1. A buffer is simply a contiguous block of computer memory that holds multiple instances of the same data type

2. Static variables are allocated at load time on the data segment

3. Dynamic variables are allocated at run time on the stack. To overflow is to flow, or fill over the top, brims, or bounds

4. Processes are divided into three regions: Text, Data, and Stack.

5. Text region is fixed by the program and includes code (instructions) and read-only data. This region corresponds to the text section of the executable file. This region is normally marked read-only and any attempt to write to it will result in a segmentation violation.

6. Data region contains initialized and uninitialized data. Static variables are stored in this region. The data region corresponds to the data-bss sections of the executable file. Its size can be changed with the brk(2) system call. If the expansion of the bss data or the user stack exhausts available memory, the process is blocked and is rescheduled to run again with a larger memory space. New memory is added between the data and stack segments.

7. A stack is a contiguous block of memory containing data. A register called the stack pointer (SP) points to the top of the stack. The bottom of the stack is at a fixed address. Its size is dynamically adjusted by the kernel at run time. The CPU implements instructions to PUSH onto and POP off of the stack.

8. The first thing a procedure must do when called is save the previous FP (so it can be restored at procedure exit). Then it copies SP into FP to create the new FP, and advances SP to reserve space for the local variables. This code is called the procedure prolog. Upon procedure exit, the stack must be cleaned up again, something called the procedure epilog.S
