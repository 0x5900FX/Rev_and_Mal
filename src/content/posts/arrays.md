---
title: "Arrays in Assembly"
date: 2026-06-01
tags: [Learning]
description: Use of array in assembly
---



Storing array data in buffer

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

lea rsp, [buff]
lea rsi, [buff]
mov eax , 1

loop:
mov [rsi] , al
inc al
add rsi , 1
int3
jmp loop

.data 
buff: .skip 4096
```



Challenge


Use 4byte register to create an array

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

lea rsp, [buff]
lea rsi, [buff]
mov eax , 1

loop:
mov [rsi] , eax
inc eax
add rsi , 4
int3
jmp loop

.data 
buff: .skip 4096
```



Reading in string

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

lea rsp , [datax]
lea rsi , [datax]


lea rdi , [rsi + 50]
mov [rdi] , rsi
add rdi , 8
lea rsi , [data2]
mov [rdi] , rsi

.data 
datax: .asciz "hello world" , "fuck of" 
data2: .asciz "teteo"

```

