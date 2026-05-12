---
title: "Disassembly & x86"
date: 2026-05-11
tags: [Learning]
description: S
---


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

