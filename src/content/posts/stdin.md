---
title: "Reading input from Stdin"
date: 2026-05-25
tags: [Learning]
description: utilizing syscall for stdin.
---

Reading using syscall

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

lea rsp , [buff]

mov rax , 0 ;#read
mov rdi , 0 ;# stdin
lea rsi , [buff]  ;# address
mov rdx , 4096 
syscall

mov rdx , rax    ;#  out arhument from input syscall containing nof of charactere
mov rax , 1      ;#  write
mov rdi , 1          ;# terminal stdout
lea rsi , [buff]     ;# location mem
syscall

mov rax , 60
xor rdx , rdx
syscall

.data 
buff: .skip 4096, 0xff

```


Continious reading from terminal
```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

_read:
mov rax , 0 ;#read
mov rdi , 0 ;# stdin
lea rsi , [buff]
mov rdx , 4096
syscall


_write:
mov rdx , rax 
mov rax , 1
mov rdi , 1
lea rsi , [buff]
syscall

jmp _read

mov rax , 60
xor rdx , rdx
syscall

```



Challenge
Adding new line to the text in infinite input from stdin.

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

_read:
mov rax , 0 ;#read
mov rdi , 0 ;# stdin
lea rsi , [buff]
mov rdx , 4096
syscall

mov byte ptr [rsi + rax ] , '\n'

_write:
mov rdx , rax 
inc rdx
mov rax , 1
mov rdi , 1
lea rsi , [buff]
syscall

jmp _read

mov rax , 60
xor rdx , rdx
syscall




```
