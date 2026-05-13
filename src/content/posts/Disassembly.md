---
title: "Disassembly & learning x86"
date: 2026-05-11
tags: [Learning]
description: S
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