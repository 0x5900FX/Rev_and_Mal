---
title: "Comparing My Hand-Written Assembly with GCC's Output"
date: 2026-06-02
tags: [Learning]
description: I wrote a number converter in Assembly, then wrote the same thing in C. Here is what the compiler did differently under the hood.
---

I built a base converter in pure x86-64 Assembly using Linux syscalls. Then, I wrote the same logic in C, compiled it with gcc -O0, and disassembled it with objdump. The results were eye-opening.

So we have the C code for  converter.

```C
#include <stdio.h>
void print_binary(int num) ;


int main(void) {

    int input;

    printf("Enter num from 1 - 225 : ");

    scanf("%d",&input);


    printf("Decimal :  %d \n", input);

    printf("Hex :  0x%x \n", input);

    print_binary(input);

}

void print_binary(int num) {
    char buffer[33];
    int i = 0 ;


    if (num == 0 ) {
        printf("0\n");
        return;
    }
    while (num > 0) {
        int remainder = num % 2;
        buffer[i] = remainder + '0';
        i++;
        num = num / 2;
    }


    for (int j = i -1 ; j >= 0 ; j--) {
        printf("%c", buffer[j]);
    }
    printf("\n");

}

```

Let's compile the C code to asm code.

:/tmp/test$ gcc -O0 -masm=intel -o converter converter.c 
:/tmp/test$ objdump  -d -M Assemblyintel converter | less


here is the code from objdump 


objdump  -d -M intel converter 

converter:     file format elf64-x86-64


Disassembly of section .init:

``` Asm
0000000000001000 <_init>:
    1000:	f3 0f 1e fa          	endbr64
    1004:	48 83 ec 08          	sub    rsp,0x8
    1008:	48 8b 05 d9 2f 00 00 	mov    rax,QWORD PTR [rip+0x2fd9]        # 3fe8 <__gmon_start__>
    100f:	48 85 c0             	test   rax,rax
    1012:	74 02                	je     1016 <_init+0x16>
    1014:	ff d0                	call   rax
    1016:	48 83 c4 08          	add    rsp,0x8
    101a:	c3                   	ret

Disassembly of section .plt:

0000000000001020 <.plt>:
    1020:	ff 35 7a 2f 00 00    	push   QWORD PTR [rip+0x2f7a]        # 3fa0 <_GLOBAL_OFFSET_TABLE_+0x8>
    1026:	ff 25 7c 2f 00 00    	jmp    QWORD PTR [rip+0x2f7c]        # 3fa8 <_GLOBAL_OFFSET_TABLE_+0x10>
    102c:	0f 1f 40 00          	nop    DWORD PTR [rax+0x0]
    1030:	f3 0f 1e fa          	endbr64
    1034:	68 00 00 00 00       	push   0x0
    1039:	e9 e2 ff ff ff       	jmp    1020 <_init+0x20>
    103e:	66 90                	xchg   ax,ax
    1040:	f3 0f 1e fa          	endbr64
    1044:	68 01 00 00 00       	push   0x1
    1049:	e9 d2 ff ff ff       	jmp    1020 <_init+0x20>
    104e:	66 90                	xchg   ax,ax
    1050:	f3 0f 1e fa          	endbr64
    1054:	68 02 00 00 00       	push   0x2
    1059:	e9 c2 ff ff ff       	jmp    1020 <_init+0x20>
    105e:	66 90                	xchg   ax,ax
    1060:	f3 0f 1e fa          	endbr64
    1064:	68 03 00 00 00       	push   0x3
    1069:	e9 b2 ff ff ff       	jmp    1020 <_init+0x20>
    106e:	66 90                	xchg   ax,ax
    1070:	f3 0f 1e fa          	endbr64
    1074:	68 04 00 00 00       	push   0x4
    1079:	e9 a2 ff ff ff       	jmp    1020 <_init+0x20>
    107e:	66 90                	xchg   ax,ax

Disassembly of section .plt.got:

0000000000001080 <__cxa_finalize@plt>:
    1080:	f3 0f 1e fa          	endbr64
    1084:	ff 25 6e 2f 00 00    	jmp    QWORD PTR [rip+0x2f6e]        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    108a:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

Disassembly of section .plt.sec:

0000000000001090 <putchar@plt>:
    1090:	f3 0f 1e fa          	endbr64
    1094:	ff 25 16 2f 00 00    	jmp    QWORD PTR [rip+0x2f16]        # 3fb0 <putchar@GLIBC_2.2.5>
    109a:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

00000000000010a0 <puts@plt>:
    10a0:	f3 0f 1e fa          	endbr64
    10a4:	ff 25 0e 2f 00 00    	jmp    QWORD PTR [rip+0x2f0e]        # 3fb8 <puts@GLIBC_2.2.5>
    10aa:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

00000000000010b0 <__stack_chk_fail@plt>:
    10b0:	f3 0f 1e fa          	endbr64
    10b4:	ff 25 06 2f 00 00    	jmp    QWORD PTR [rip+0x2f06]        # 3fc0 <__stack_chk_fail@GLIBC_2.4>
    10ba:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

00000000000010c0 <printf@plt>:
    10c0:	f3 0f 1e fa          	endbr64
    10c4:	ff 25 fe 2e 00 00    	jmp    QWORD PTR [rip+0x2efe]        # 3fc8 <printf@GLIBC_2.2.5>
    10ca:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

00000000000010d0 <__isoc23_scanf@plt>:
    10d0:	f3 0f 1e fa          	endbr64
    10d4:	ff 25 f6 2e 00 00    	jmp    QWORD PTR [rip+0x2ef6]        # 3fd0 <__isoc23_scanf@GLIBC_2.38>
    10da:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]

Disassembly of section .text:

00000000000010e0 <_start>:
    10e0:	f3 0f 1e fa          	endbr64
    10e4:	31 ed                	xor    ebp,ebp
    10e6:	49 89 d1             	mov    r9,rdx
    10e9:	5e                   	pop    rsi
    10ea:	48 89 e2             	mov    rdx,rsp
    10ed:	48 83 e4 f0          	and    rsp,0xfffffffffffffff0
    10f1:	50                   	push   rax
    10f2:	54                   	push   rsp
    10f3:	45 31 c0             	xor    r8d,r8d
    10f6:	31 c9                	xor    ecx,ecx
    10f8:	48 8d 3d ca 00 00 00 	lea    rdi,[rip+0xca]        # 11c9 <main>
    10ff:	ff 15 d3 2e 00 00    	call   QWORD PTR [rip+0x2ed3]        # 3fd8 <__libc_start_main@GLIBC_2.34>
    1105:	f4                   	hlt
    1106:	66 2e 0f 1f 84 00 00 	cs nop WORD PTR [rax+rax*1+0x0]
    110d:	00 00 00 

0000000000001110 <deregister_tm_clones>:
    1110:	48 8d 3d f9 2e 00 00 	lea    rdi,[rip+0x2ef9]        # 4010 <__TMC_END__>
    1117:	48 8d 05 f2 2e 00 00 	lea    rax,[rip+0x2ef2]        # 4010 <__TMC_END__>
    111e:	48 39 f8             	cmp    rax,rdi
    1121:	74 15                	je     1138 <deregister_tm_clones+0x28>
    1123:	48 8b 05 b6 2e 00 00 	mov    rax,QWORD PTR [rip+0x2eb6]        # 3fe0 <_ITM_deregisterTMCloneTable>
    112a:	48 85 c0             	test   rax,rax
    112d:	74 09                	je     1138 <deregister_tm_clones+0x28>
    112f:	ff e0                	jmp    rax
    1131:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]
    1138:	c3                   	ret
    1139:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

0000000000001140 <register_tm_clones>:
    1140:	48 8d 3d c9 2e 00 00 	lea    rdi,[rip+0x2ec9]        # 4010 <__TMC_END__>
    1147:	48 8d 35 c2 2e 00 00 	lea    rsi,[rip+0x2ec2]        # 4010 <__TMC_END__>
    114e:	48 29 fe             	sub    rsi,rdi
    1151:	48 89 f0             	mov    rax,rsi
    1154:	48 c1 ee 3f          	shr    rsi,0x3f
    1158:	48 c1 f8 03          	sar    rax,0x3
    115c:	48 01 c6             	add    rsi,rax
    115f:	48 d1 fe             	sar    rsi,1
    1162:	74 14                	je     1178 <register_tm_clones+0x38>
    1164:	48 8b 05 85 2e 00 00 	mov    rax,QWORD PTR [rip+0x2e85]        # 3ff0 <_ITM_registerTMCloneTable>
    116b:	48 85 c0             	test   rax,rax
    116e:	74 08                	je     1178 <register_tm_clones+0x38>
    1170:	ff e0                	jmp    rax
    1172:	66 0f 1f 44 00 00    	nop    WORD PTR [rax+rax*1+0x0]
    1178:	c3                   	ret
    1179:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

0000000000001180 <__do_global_dtors_aux>:
    1180:	f3 0f 1e fa          	endbr64
    1184:	80 3d 85 2e 00 00 00 	cmp    BYTE PTR [rip+0x2e85],0x0        # 4010 <__TMC_END__>
    118b:	75 2b                	jne    11b8 <__do_global_dtors_aux+0x38>
    118d:	55                   	push   rbp
    118e:	48 83 3d 62 2e 00 00 	cmp    QWORD PTR [rip+0x2e62],0x0        # 3ff8 <__cxa_finalize@GLIBC_2.2.5>
    1195:	00 
    1196:	48 89 e5             	mov    rbp,rsp
    1199:	74 0c                	je     11a7 <__do_global_dtors_aux+0x27>
    119b:	48 8b 3d 66 2e 00 00 	mov    rdi,QWORD PTR [rip+0x2e66]        # 4008 <__dso_handle>
    11a2:	e8 d9 fe ff ff       	call   1080 <__cxa_finalize@plt>
    11a7:	e8 64 ff ff ff       	call   1110 <deregister_tm_clones>
    11ac:	c6 05 5d 2e 00 00 01 	mov    BYTE PTR [rip+0x2e5d],0x1        # 4010 <__TMC_END__>
    11b3:	5d                   	pop    rbp
    11b4:	c3                   	ret
    11b5:	0f 1f 00             	nop    DWORD PTR [rax]
    11b8:	c3                   	ret
    11b9:	0f 1f 80 00 00 00 00 	nop    DWORD PTR [rax+0x0]

00000000000011c0 <frame_dummy>:
    11c0:	f3 0f 1e fa          	endbr64
    11c4:	e9 77 ff ff ff       	jmp    1140 <register_tm_clones>

00000000000011c9 <main>:
    11c9:	f3 0f 1e fa          	endbr64
    11cd:	55                   	push   rbp
    11ce:	48 89 e5             	mov    rbp,rsp
    11d1:	48 83 ec 10          	sub    rsp,0x10
    11d5:	64 48 8b 04 25 28 00 	mov    rax,QWORD PTR fs:0x28
    11dc:	00 00 
    11de:	48 89 45 f8          	mov    QWORD PTR [rbp-0x8],rax
    11e2:	31 c0                	xor    eax,eax
    11e4:	48 8d 05 19 0e 00 00 	lea    rax,[rip+0xe19]        # 2004 <_IO_stdin_used+0x4>
    11eb:	48 89 c7             	mov    rdi,rax
    11ee:	b8 00 00 00 00       	mov    eax,0x0
    11f3:	e8 c8 fe ff ff       	call   10c0 <printf@plt>
    11f8:	48 8d 45 f4          	lea    rax,[rbp-0xc]
    11fc:	48 8d 15 1b 0e 00 00 	lea    rdx,[rip+0xe1b]        # 201e <_IO_stdin_used+0x1e>
    1203:	48 89 c6             	mov    rsi,rax
    1206:	48 89 d7             	mov    rdi,rdx
    1209:	b8 00 00 00 00       	mov    eax,0x0
    120e:	e8 bd fe ff ff       	call   10d0 <__isoc23_scanf@plt>
    1213:	8b 45 f4             	mov    eax,DWORD PTR [rbp-0xc]
    1216:	48 8d 15 04 0e 00 00 	lea    rdx,[rip+0xe04]        # 2021 <_IO_stdin_used+0x21>
    121d:	89 c6                	mov    esi,eax
    121f:	48 89 d7             	mov    rdi,rdx
    1222:	b8 00 00 00 00       	mov    eax,0x0
    1227:	e8 94 fe ff ff       	call   10c0 <printf@plt>
    122c:	8b 45 f4             	mov    eax,DWORD PTR [rbp-0xc]
    122f:	48 8d 15 fb 0d 00 00 	lea    rdx,[rip+0xdfb]        # 2031 <_IO_stdin_used+0x31>
    1236:	89 c6                	mov    esi,eax
    1238:	48 89 d7             	mov    rdi,rdx
    123b:	b8 00 00 00 00       	mov    eax,0x0
    1240:	e8 7b fe ff ff       	call   10c0 <printf@plt>
    1245:	8b 45 f4             	mov    eax,DWORD PTR [rbp-0xc]
    1248:	89 c7                	mov    edi,eax
    124a:	e8 1b 00 00 00       	call   126a <print_binary>
    124f:	b8 00 00 00 00       	mov    eax,0x0
    1254:	48 8b 55 f8          	mov    rdx,QWORD PTR [rbp-0x8]
    1258:	64 48 2b 14 25 28 00 	sub    rdx,QWORD PTR fs:0x28
    125f:	00 00 
    1261:	74 05                	je     1268 <main+0x9f>
    1263:	e8 48 fe ff ff       	call   10b0 <__stack_chk_fail@plt>
    1268:	c9                   	leave
    1269:	c3                   	ret

000000000000126a <print_binary>:
    126a:	f3 0f 1e fa          	endbr64
    126e:	55                   	push   rbp
    126f:	48 89 e5             	mov    rbp,rsp
    1272:	48 83 ec 50          	sub    rsp,0x50
    1276:	89 7d bc             	mov    DWORD PTR [rbp-0x44],edi
    1279:	64 48 8b 04 25 28 00 	mov    rax,QWORD PTR fs:0x28
    1280:	00 00 
    1282:	48 89 45 f8          	mov    QWORD PTR [rbp-0x8],rax
    1286:	31 c0                	xor    eax,eax
    1288:	c7 45 c4 00 00 00 00 	mov    DWORD PTR [rbp-0x3c],0x0
    128f:	83 7d bc 00          	cmp    DWORD PTR [rbp-0x44],0x0
    1293:	75 4a                	jne    12df <print_binary+0x75>
    1295:	48 8d 05 a3 0d 00 00 	lea    rax,[rip+0xda3]        # 203f <_IO_stdin_used+0x3f>
    129c:	48 89 c7             	mov    rdi,rax
    129f:	e8 fc fd ff ff       	call   10a0 <puts@plt>
    12a4:	eb 72                	jmp    1318 <print_binary+0xae>
    12a6:	8b 55 bc             	mov    edx,DWORD PTR [rbp-0x44]
    12a9:	89 d0                	mov    eax,edx
    12ab:	c1 f8 1f             	sar    eax,0x1f
    12ae:	c1 e8 1f             	shr    eax,0x1f
    12b1:	01 c2                	add    edx,eax
    12b3:	83 e2 01             	and    edx,0x1
    12b6:	29 c2                	sub    edx,eax
    12b8:	89 55 cc             	mov    DWORD PTR [rbp-0x34],edx
    12bb:	8b 45 cc             	mov    eax,DWORD PTR [rbp-0x34]
    12be:	83 c0 30             	add    eax,0x30
    12c1:	89 c2                	mov    edx,eax
    12c3:	8b 45 c4             	mov    eax,DWORD PTR [rbp-0x3c]
    12c6:	48 98                	cdqe
    12c8:	88 54 05 d0          	mov    BYTE PTR [rbp+rax*1-0x30],dl
    12cc:	83 45 c4 01          	add    DWORD PTR [rbp-0x3c],0x1
    12d0:	8b 45 bc             	mov    eax,DWORD PTR [rbp-0x44]
    12d3:	89 c2                	mov    edx,eax
    12d5:	c1 ea 1f             	shr    edx,0x1f
    12d8:	01 d0                	add    eax,edx
    12da:	d1 f8                	sar    eax,1
    12dc:	89 45 bc             	mov    DWORD PTR [rbp-0x44],eax
    12df:	83 7d bc 00          	cmp    DWORD PTR [rbp-0x44],0x0
    12e3:	7f c1                	jg     12a6 <print_binary+0x3c>
    12e5:	8b 45 c4             	mov    eax,DWORD PTR [rbp-0x3c]
    12e8:	83 e8 01             	sub    eax,0x1
    12eb:	89 45 c8             	mov    DWORD PTR [rbp-0x38],eax
    12ee:	eb 18                	jmp    1308 <print_binary+0x9e>
    12f0:	8b 45 c8             	mov    eax,DWORD PTR [rbp-0x38]
    12f3:	48 98                	cdqe
    12f5:	0f b6 44 05 d0       	movzx  eax,BYTE PTR [rbp+rax*1-0x30]
    12fa:	0f be c0             	movsx  eax,al
    12fd:	89 c7                	mov    edi,eax
    12ff:	e8 8c fd ff ff       	call   1090 <putchar@plt>
    1304:	83 6d c8 01          	sub    DWORD PTR [rbp-0x38],0x1
    1308:	83 7d c8 00          	cmp    DWORD PTR [rbp-0x38],0x0
    130c:	79 e2                	jns    12f0 <print_binary+0x86>
    130e:	bf 0a 00 00 00       	mov    edi,0xa
    1313:	e8 78 fd ff ff       	call   1090 <putchar@plt>
    1318:	48 8b 45 f8          	mov    rax,QWORD PTR [rbp-0x8]
    131c:	64 48 2b 04 25 28 00 	sub    rax,QWORD PTR fs:0x28
    1323:	00 00 
    1325:	74 05                	je     132c <print_binary+0xc2>
    1327:	e8 84 fd ff ff       	call   10b0 <__stack_chk_fail@plt>
    132c:	c9                   	leave
    132d:	c3                   	ret

Disassembly of section .fini:

0000000000001330 <_fini>:
    1330:	f3 0f 1e fa          	endbr64
    1334:	48 83 ec 08          	sub    rsp,0x8
    1338:	48 83 c4 08          	add    rsp,0x8
    133c:	c3                   	ret
zephyr@0xF34R:/tmp/test$ 


```



And here is the asm code that we wrote
```Assembly
.intel_syntax noprefix
.text
.global main
main:
    mov %rbp, %rsp #for correct debugging
   

   call read_str
   
   lea rdi ,[welcome_usr]
   call print_str
   
   ;# lea rdi ,[user_inp]
   ;#  call print_str
   
   lea rdi , [user_inp]
   call str_len
   
   mov rcx , rax
   lea rsi , [user_inp]
   
   call ston
   mov  [number] , rax
   
    mov rdi , [number]
      call print_int
   
   
   
   call exit
  
   
   
   
   
  
   
   
   read_str:
    mov rax , 0
    mov rdi , 0
    lea rsi , [user_inp]
    lea rdx , [u_len]
    syscall
    mov byte ptr [user_inp + rax ] , 0
    ret 
    
    
      print_str:
        push rdi
        call str_len
        pop rdi
        
        mov rdx, rax
        mov rsi , rdi
        
        mov rax , 1
        mov rdi , 1
        
        syscall 
        ret
    
    str_len:
        xor rcx,rcx
        mov rsi ,rdi
        
        str_len.loop:
        mov bl , [rsi]
        cmp bl , 0
        je str_len.exit
        inc rsi
        inc rcx
        jmp str_len.loop
        
        str_len.exit:
        mov rax, rcx
        ret
        
    ston:
        push rbx
        xor rax , rax 
        xor rbx , rbx
        
        ston.loop:
        cmp rcx , 0
        jle ston.done
        movzx rbx , byte ptr [rsi]
        cmp bl , '0'
        jb ston.done
        cmp bl , '9'
        ja ston.done
        imul rax , rax , 10
        sub bl , '0'
        add rax , rbx
        inc rsi
        dec rcx
        jmp ston.loop
        
        ston.done:
        pop rbx
        ret
    
    print_int:
        mov rax , rdi
        lea rsi , [int_buffer + 20]
        mov byte ptr [rsi] , 0
        mov rcx , 10
        mov rbx , rsi
        
        cmp rax , 0
        jne print_int.loop
        
        mov byte ptr [rsi -1 ] ,'0'
        lea rdi , [rsi -1]
        call print_str
        ret
        
        
        print_int.loop:
        xor rdx , rdx
        div rcx
        add dl , '0'
        dec rsi
        mov [rsi] , dl
        cmp rax , 0
        jnz print_int.loop
        
        mov rdi , rsi
        call print_str
        ret
                
    
  exit:
   mov rax , 60
   xor rdi , rdi
   syscall  
   
 .data
 user_inp: .skip 128 
 u_len = . - user_inp
 
 welcome_usr: .asciz "Hello user. Enter no from 1-225\n"
 wel_len = . - welcome_usr
 
 number: .quad 0
 
 int_buffer: .skip 1080
 
 ```
 
## Obvious diffrences

- Data section

In my code of asm we can just declare the .data and we can store variables
where as 

C store the data in stack - A temp workspace that grows & shrinks

```
user_inp: .skip 128
number: .quad 0


vs 

push rbp
mov rbp , rsp
sub rsp , 0x50

```

- Div function

I used div to convert numbers but compiler uses `and` & `shr` function which makes my output a whole mess.

Why didn't the compiler use div? Because div is slow! The CPU takes dozens of clock cycles to execute a division. But bitwise operations like and and shr take exactly 1 clock cycle.

By shifting the bits to the right by 1 (shr), the CPU effectively divides the number by 2 instantly. By using and 1, it masks out everything except the last bit, instantly giving the remainder.

```
div rcx 

vs 

and edx , 0x1

```


Conclusion : Which one is better?

looking at objdump data C feels lika  `garbage` as it uses the pipeline to make the process faster. But that garbage is hard to understand however is every optimiezed machine code


maybe reading malware can be a mess like this where malware creator uses their own malware and don't rely on other libs. Understanding this raw logic helped me gain more knowledge on this field.