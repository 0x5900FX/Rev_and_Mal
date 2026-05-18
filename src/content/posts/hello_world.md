---
title: "Hello World in Assembly"
date: 2026-05-15
tags: [Learning]
description: Learning Assembly in a new way
---

## How would you tell to print the data ? 
printf("Hello, world!");

"Hello, world" -> goes into .data segment

Kernel call / System call = SYSCALL

printf:

1. Format
2. Tell kernel WHAT we want to do (write)
3. Tell kernel WHERE we want to write to
4. Tell kernel WHICH address (of string)
5. Tell kernel HOW MANY bytes to write

SYSCALL(WRITE, TERMINAL,0x4000 , 13)

---

Which register to populate??
```
Role	Register
Syscall ID	%rax
Argument 1	%rdi
Argument 2	%rsi
Argument 3	%rdx
Argument 4	%r10 (Note: standard functions use %rcx)
Argument 5	%r8
Argument 6	%r9
Return Value	%rax

output:
SYSCALL(WRITE, TERMINAL, ADDRESS , LEN)

in C
Write(terminal ,addr, len);


```

# SYSCALL 
  The system call is the fundamental interface between an application and the Linux kernel. Defines what services to execute.

Syscall table contains of important syscall num to define what operation to execute. 
can be founded on https://filippo.io/linux-syscall-table/

```
The return value is placed in %rax.

%rax	Name	Manual	Entry point
0	read	read(2)	sys_read
1	write	write(2)	sys_write
2	open	open(2)	sys_open
3	close	close(2)	sys_close
4	stat	stat(2)	sys_newstat
5	fstat	fstat(2)	sys_newfstat
6	lstat	lstat(2)	sys_newlstat
7	poll	poll(2)	sys_poll
8	lseek	lseek(2)	sys_lseek
```
```
Standard File Descriptors
Every new process starts with three default file descriptors inherited from its parent process (typically the shell): 

FD 	Name	Default Purpose	Default Source/Sink
0	stdin	Standard Input	Keyboard
1	stdout	Standard Output	Terminal/Console
2	stderr	Standard Error	Terminal/Console
```


Writing "hello world" to output

```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:


mov rax , 1 #syscall write
mov rdi , 1     # fd = stdout
lea rsi , [hwlodata]        # text mem ADDRESS
mov rdx , 11   # no of string
syscall


.data 
hwlodata: .byte 'h','e','l','l','o',' ','w','o','r','l','d'
```

Thus we can print Hello would 


Another way to do it 
```
;#---------------------
;#  GNU Assembler file
;#  Syscall Hello World
;#---------------------
.intel_syntax noprefix
.global _start
.text
_start:


mov rax , 1     #syscall write
mov rdi ,1      #fd = stdout
lea rsi , [a_helo]  #location of string
lea rdx , [hellolen]    #string length
syscall     

mov rax , 60        #syscall code for exit
xor rdi , rdi    # clear rdi -> exit code = 0 (success)
syscall         # we can exit without segmentation fault 

.data
a_helo: .ascii "\n hello mate \n"
hellolen  = . - a_helo 
```

Using syscall code 60. We can exit with msg `program terminated with Exit(1)`
rather than `Program terminated with Exit(139) due to signal SIGSEGV: Invalid memory reference.`


```
rax = syscall number
rdi = 1st argument
rsi = 2nd argument
rdx = 3rd argument
r10 = 4th argument
r8  = 5th argument
r9  = 6th argument
syscall
```


## Challenge 
Output 2 string to terminal and exit with code 99 and then code 0

```
.intel_syntax noprefix
.global _start
.text
_start:


mov rax , 1
mov rdi , 1
lea rsi ,[string_data]
lea rdx , [count]
syscall

mov rax , 1 
mov rdi ,1 
lea rsi , [string_sec]
lea rdx , [count]
syscall

mov rax , 60
mov rdi , 99
syscall



.data
string_data: .ascii "hello gamers\n"
count = . - string_data
string_sec: .ascii "what's up ??"
count = . - string_sec

```

```

mov rax , 60
xor rdi , rdi
syscall

```
We can do this to exit with code 0.




## Using jump in Assembly.


`JMP` command is use to memory lable that we mentioned.
Like a function in a program JMP is used to jump forth and back bet'n two lables

```

print:
mov rax , 1 
mov rdi , 1
lea rsi , [string_data]
lea rdx , [count_1]
syscall

exit:
mov rax , 60
xor rdi , rdi 
syscall
```

It can be donw with `lable` name and :

```
jmp print

exit:
mov rax , 60
xor rdi , rdi 
syscall


print:
mov rax , 1 
mov rdi , 1
lea rsi , [string_data]
lea rdx , [count_1]
syscall
jmp exit


Running 

jmp print
jmp exit 

without jmp exit in print will led to seg fault. 

So we use stack there..
It uses two keyword. RET & CALL

Example to show how to use `jmp`

```

Dynamic stack 

```
We stored mem address for exit & store at r12.
After the end of print label it'll point to r12 which contains the address for exit label.

.intel_syntax noprefix
.global _start
.text
_start:


lea r12 , [return]
jmp print

return:
jmp exit 

exit:
mov rax , 60
xor rdi , rdi 
syscall


print:
mov rax , 1 
mov rdi , 1
lea rsi , [string_data]
lea rdx , [count_1]
syscall
jmp r12


.data
string_data: .ascii "hello gamers\n"
count_1 = . - string_data
string_sec: .ascii "what's up ??"
count_2 = . - string_sec
```

Dynamic return mechanism using a preserved register:

The code implements a manual indirect jump where r12 serves as a stored return address:

lea r12, [return] → loads the absolute address of return into r12

jmp print → tail-calls the print routine (no stack frame)

jmp r12 in print → indirect jump to the pre-stored address in r12

This is effectively a custom calling convention where:

Caller stores return address in a callee-saved register (r12)

Callee jumps back via jmp r12 instead of ret


Running program label inside another one
```

.intel_syntax noprefix
.global _start
.text
_start:


lea r12 , [return]
jmp print

return:
jmp exit 

exit:
mov rax , 60
xor rdi , rdi 
syscall


print:
mov rax , 1 
mov rdi , 1
lea rsi , [string_data]
lea rdx , [count_1]
syscall
jmp print_another_word

print_another_word:
mov rax , 1 
mov rdi , 1
lea rsi , [string_sec]
lea rdx , [count_2 ]
syscall
jmp r12

.data
string_data: .ascii "hello gamers\n"
count_1 = . - string_data
string_sec: .ascii "what's up ??"
count_2 = . - string_sec
```

##  CallStack


...