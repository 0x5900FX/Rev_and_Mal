---
title: "Disassembly & learning x86"
date: 2026-05-11
tags: [Learning]
description: Learning Assembly as first part for diassembly.
---

So basically, We are gonna learn basic of Assembly. We are gonna use `https://x64.halb.it/` to play the ground.
we are gonna use `GNU as` to compile the file..

This is a biolerplate for asm code.


```

;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

 
```


`MOV command`
Copy data from one register to another register.

```
.intel_syntax noprefix
.global _start
.text
_start:
 
 mov rax , 10   /stores 0xa into rax 
 mov rbx , rax  
 mov rcx , 0x1000  /stores 0x1000 into rcx
  
 
 ```

 Be careful with `rsp` and  `rbp` , `rip`. `rip` cannot be modified. SO try to avoid storing data.  

```
00401000	_start:
00401000	48 c7c00a000000	mov rax, 10
00401007	48 89c3	mov rbx, rax
0040100a	48 c7c100100000	mov rcx, 0x1000
00401011	0000	add [rax], al
```

Imagine this is a memory address of our program. 
```

what would `mov rdx , [0x401000]` do?
It says mov the data stored at address [0x401000] into rdx  which is  `48 c7c00a000000` 

Register state after execution

rax : 0x0a
rbx : 0x0a
rcx : 0x1000
rdx : 0x480000000ac0c748 /We got extra value as it's 8 bytes wide. it's data is reversed based on endianess.
rsp : 0x4ffffffffed0
rbp : 0x00


```

---



Learning to add in Asm



```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

 
mov rax , 2  
add rax , 4   # rax is 6 

mov rbx , 4 	# rbx is 4
add rax , rbx 	# rax is 0x0a


Register value

rax : 0x0a

rbx : 0x04

rcx : 0x00

rdx : 0x4ffffffffff7

rsp : 0x4ffffffffed0

rbp : 0x00

```

---

Learning to subtract in `asm`

```

;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

 
mov rax , 2
add rax , 4  

mov rbx , 4
add rax , rbx

sub rax , 7 # sub 7 from rax
sub rax , 1 # sub 1 from rax


Registers

rax : 0x02

rbx : 0x04

rcx : 0x00

rdx : 0x4ffffffffff7

rsp : 0x4ffffffffed0

```


---
---


```
mov rax , 10 
mov rbx , 5
sub rax , rbx  #sub rbx from rax and store in rax.

Registers

rax : 0x05

rbx : 0x05

rcx : 0x00

rdx : 0x4ffffffffff7

```


`inc / dec `

```
_start:

mov rax , 9 
inc rax   #inc rom 9 to a -> 10
mov rbx , 9
inc rbx  #inc rom 9 to a -> 10

Registers

rax : 0x0a

rbx : 0x0a

rcx : 0x00

```


Some Exercies 
`Getting the fibonacci numbers from the lession till now`.
with only i immidiate value load



```


;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

#;! 0,1,2,3,5,8,13,21,34,55,89 
mov rax ,0
mov rbx  , rax 


inc rbx 
add rbx , rax 

mov rcx ,rbx  
mov rbp , rcx 
add rbp , rcx 

mov rsi , rbp
add rsi , rcx

mov rdi , rsi
add rdi , rbp

mov r8, rdi
add r8, rsi

mov r9, r8
add r9, rdi

```

Disassembling `Hello world`.
Using `cl.exe`.

``` C
#include <stdio.h>

int main() {
	printf("Hello , World");
}

cl.exe .\haloworld.c /Fahalloworld.asm
Microsoft (R) C/C++ Optimizing Compiler Version 19.50.35730 for x86
Copyright (C) Microsoft Corporation.  All rights reserved.

haloworld.c
Microsoft (R) Incremental Linker Version 14.50.35730.0
Copyright (C) Microsoft Corporation.  All rights reserved.

/out:haloworld.exe
haloworld.obj
```

s