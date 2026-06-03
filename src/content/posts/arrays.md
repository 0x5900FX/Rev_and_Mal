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


Tokenize in ASM

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:


lea rdi , [buf]
lea rsi , [bufsize - 1]
call readline

lea rdi , [buf]
mov byte ptr [rdi + rax] , 0

call memviewer

lea rdi , [buf]
lea rsi , [tokens]
call tokenize

lea rdi , [buf]
call _print

call _exit

_exit:
        mov rax , 60
        xor rdi, rdi
        syscall

readline:
        mov rdx, rsi
        mov rsi, rdi
        mov rax, 0
        mov rdi, 0
        syscall
        ret

_print:
        push rdi
        call slen
        pop rsi
        mov rdx, rax
        mov rax, 1
        mov rdi, 1
        syscall
        ret
      

slen:
    xor rcx, rcx
 .loop:
    cmp byte ptr [rdi + rcx], 0
    je .done
    inc rcx
    jmp .loop
 .done:
    mov rax, rcx
    ret

memviewer:
mov r14 , rsp
lea rsp , [buf] 
int3
mov rsp , r14
ret

tokenize:
 mov r15 , rsi
 tokenize.loop:
 mov [r15] , rdi
 add r15 , 8
call memviewer
ret

.data
buf: .skip 128 , 0xff
bufsize = . - buf
tokens: .skip 128
```