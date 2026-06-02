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
lea rsi , [buff]
mov rdx , 4096
syscall


mov rax , 60
xor rdx , rdx
syscall

.data 
buff: .skip 4096, 0xff

```