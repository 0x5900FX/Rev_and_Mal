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