---
title: "If else statement in Asm."
date: 2026-05-23
tags: [Learning]
description: Utilizing assembly with conditioms.
---

Using `CMP` to compare for value.

it can be defined as


**j*condition*** — Conditional jump

> These instructions are conditional jumps that are based on the status of a set of condition codes that are stored in a special register called the *machine status word*. The contents of the machine status word include information about the last arithmetic operation performed. For example, one bit of this word indicates if the last result was zero. Another indicates if the last result was negative. Based on these condition codes, a number of conditional jumps can be performed. For example, the **jz** instruction performs a jump to the specified operand label if the result of the last arithmetic operation was zero. Otherwise, control proceeds to the next instruction in sequence.A number of the conditional branches are given names that are intuitively based on the last operation performed being a special compare instruction, **cmp** (see below). For example, conditional branches such as **jle** and **jne** are based on first performing a cmp operation on the desired operands.
>
> *Syntax*
> je <label> (jump when equal)
> jne <label> (jump when not equal)
> jz <label> (jump when last result was zero)
> jg <label> (jump when greater than)
> jge <label> (jump when greater than or equal to)
> jl <label> (jump when less than)
> jle <label> (jump when less than or equal to)
>
> *Example*
>
> ```
> cmp %ebx, %eax
> jle done
> ```
>



Example

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:
mov rax , 0

;# if <condition>
cmp rax , 5
;#jne exit ;# jmp if not equal

jge exit ;# jmp if -gt or -eq to 5
;# if fails then go to running statement
inc rax 
jmp 0x00401007





;#<body>
print:
mov rax , 1 
mov rdi , 1
lea rsi , [str]
lea rdx , [strlen]
syscall
;#end


exit:
mov rax , 60
xor rdi , rdi
syscall

.data
str: .ascii "the rax is 3!"
strlen = . - str

```

---

Challenge:
Writing our own for loop in assembly

for (int i = 0 ; i < 10 ; i++){

bodyy...

}


```code
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:

;#for (int i = 0; i < 10; i++) {
;#    print();
;#}

mov r15 , 0 
compare:
cmp r15 , 10
jge exit
inc r15
call print
jmp compare


exit:
mov rax , 60
xor rdi , rdi
syscall 

print:
mov rax , 1 
mov rdi , 1
lea rsi , [str]
lea rdx , [len]
syscall
ret

.data
str: .ascii "New data \n"
len = . - str

```
