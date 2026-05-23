---
title: "Writing our printf function in asm"
date: 2026-05-23
tags: [Learning]
description: Utilizing assembly to create our function.
---



## printf

```
#include <stdio.h>

int main(){

    char *c = "hello world\n";
    printf(c);
    printf("Hello world");
    return 0
}
```

We'll try to recreate this in assembly

# x86_64 (64-bit) Syscall Table

Compiled from Linux 5.4.0 headers.

| NR | Syscall Name | References | `%rax` | arg0 (`%rdi`) | arg1 (`%rsi`) | arg2 (`%rdx`) | arg3 (`%r10`) | arg4 (`%r8`) | arg5 (`%r9`) |
|----|---------------|------------|--------|----------------|----------------|----------------|----------------|---------------|---------------|
| 0  | `read`  | `man/`, `cs/` | `0x00` | `unsigned int fd` | `char *buf` | `size_t count` | - | - | - |
| 1  | `write` | `man/`, `cs/` | `0x01` | `unsigned int fd` | `const char *buf` | `size_t count` | - | - | - |
| 2  | `open`  | `man/`, `cs/` | `0x02` | `const char *filename` | `int flags` | `umode_t mode` | - | - | - |
| 3  | `close` | `man/`, `cs/` | `0x03` | `unsigned int fd` | - | - | - | - | - |

Syscall manuals 


### This version is more static but will make it dynamic with a lil change in it. Like passing the rdi with out own string .
```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

call print
call _exit

_exit:
mov rax , 60
xor rdi , rdi
syscall

print:
lea rdi , [s1]
call str_func

mov rdx , rax
mov rax ,1  
mov rdi , 1
lea rsi, [s1]
syscall
ret

str_func:
mov rsi , rdi

loop:
mov bl , [rsi]
cmp bl , 0
je func_exit
inc rsi
inc rcx
jmp loop

func_exit:
mov rax, rcx
int3
ret



.data
s1 : .asciz "Ahoy there!"

```


Here we made some changes like stattically deciding the string from s1 in every function call 
We change it to one call before function we can make it to which data to print

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:


lea rdi , [s2]
call print
call _exit

_exit:
mov rax , 60
xor rdi , rdi
syscall

print:
call str_func

mov rdx , rax
mov rsi , rdi

mov rax ,1  
mov rdi , 1
syscall
ret

str_func:
mov rsi , rdi

loop:
mov bl , [rsi]
cmp bl , 0
je func_exit
inc rsi
inc rcx
jmp loop

func_exit:
mov rax, rcx
int3
ret



.data
s1 : .asciz "Ahoy there!"
s2 : .asciz "fuckiff there"
```
