---
title: "Multiply & Division in Asm"
date: 2026-05-24
tags: [Learning]
description: learning multiply & division in Asm.
---


MUL & IMUL

There are two instructions for multiplying binary data. The MUL (Multiply) instruction handles unsigned data and the IMUL (Integer Multiply) handles signed data. Both instructions affect the Carry and Overflow flag.

We use `MUL` for multuply -> signed operation
And also `IMUL` for multiply  -> unsigned operation


mul uses `RAX` & `RDX` register to process & store the data
like
```

mov rax , 2
mov rdi , 2

mul rdi

;# rax is now 2*2 -> 4 
```

We can use `IMUL` to perform multipication too
```
mov rax , 2
mov rdi , 2


imul rax , rdi
```

We can do division using `DIV`. Before doing any division operation we need to clear rdx to 0 as it is also a part

```
div and idiv use fixed registers, similar to mul.

Registers Used

For 64-bit division:

Dividend is stored in RDX:RAX
RDX → high 64 bits
RAX → low 64 bits

Divisor is the operand provided to div or idiv

After division:

Quotient is stored in RAX
Remainder is stored in RDX
```



```
mov rax, 100
xor rdx, rdx
mov rcx, 7

div rcx

Result:

RAX = 14    ; quotient
RDX = 2     ; remainder
```

```

mov rdx , 0 
mov rax , 9
mov rbx, 4
div rbx

rax  - 02
rdx  - 01

```

In x86-64 assembly, CQO is commonly used right before IDIV when dividing signed 64-bit integers.

What CQO does?

CQO = Convert Quadword to Octoword

It sign-extends:

`RAX → into RDX:RAX`cd 

Meaning:

RAX contains the 64-bit dividend
CQO fills RDX with either all 0s or all 1s depending on the sign of RAX

This prepares the required 128-bit signed dividend for IDIV.
```
Example
mov     rax, -100
cqo                 ; sign-extend RAX into RDX:RAX
mov     rbx, 7
idiv    rbx

After division:

Quotient → RAX
Remainder → RDX
```

Converting `Int` to `String` using already learned data.


```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

mov rax , 21
add rax,  '0' ;# 48
mov [buf] , rax
lea rdi , [buf]
call print

exit:
mov rax , 60
xor rdi , rdi
syscall


_itoa:
mov rbx , 10
mov rax , rdi 
lea rdi , [buff+32]
;# mov dl , '\0'
;# mov [rdi] , dl
this and this do the same

;#mov byte ptr [rdi] , '\0'



print:
push rdi
call str_func  ;# here even if the fucntion change the value of rdi then also it'll work as it's saved in stack
pop rdi

mov rdx , rax
mov rsi , rdi

mov rax ,1  
mov rdi , 1
syscall
int3
ret

str_func:
xor rcx , rcx
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
s1: .asciz "hello mate"
buf: .skip 1024
```