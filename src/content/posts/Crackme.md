---
title: "My first CrackMe: Solving pipedown's I need to be honest"
date: 2026-06-06
tags: [Learning]
description: Stepping up my Reverse Engineering game. 
---

Author:
pipedown

Language:
Assembler

Upload:
2026-04-01 16:12

Platform:
Unix/linux etc.

Difficulty:
1.7 Rate!

Quality:
4.7 Rate!


Running the program. We are welcomed with this intro.
```
================================================
   REVERSE ENGINEERING CHALLENGE v3.14159
================================================

[*] This binary contains 3 hidden flags
[*] Your mission:
    1. Bypass the password verification
    2. Decrypt the 3 flags from memory

[>] Enter password: 

```


So this binary has to find the hidden 3 flags that author has hidden.

![Rev eng](/images/re1.png)

Observing the binary on `ida` we can see that it uses password to verify and unlock the flags.

Then i observed the data section which used `actual password` acryonyms to compare for passwords. It was in byte form. If we extract quad word from it. We can get the password.

Which is ||Hidden text||