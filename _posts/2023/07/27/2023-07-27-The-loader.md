---
date: 2023-07-27 20:16:23 +02:00
categories: [SecurityValley, Reversing]
tags: [Reversing, CTF, SecurityValley]
---
This is a writeup of the reversing challenge The Loader from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 3, Score: 30
# Premise
I've seen that in malware before. There must be something inside

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/the_loader](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/the_loader)
## Challenge files:
crackme-03
# Observations
Simply running the program gives us the following:
```
0.\0/
/ \

Wouldn't it be cool to be able to look into the memory?
```
and
```
1./0\
/ \

Wouldn't it be cool to be able to look into the memory?
```
the figure alternates between these two states and the counter to the left of it counts incrementally from 0 to 10.

# Solution
If we follow the advice of the program itself, we can try looking for something in memory (during runtime).
To do this, we first load the program into [gdb](https://www.sourceware.org/gdb/). 
I personally use gdb with a [dashboard](https://github.com/cyrus-and/gdb-dashboard) made by [cyrus-and](https://github.com/cyrus-and), and its not a requirement, but I consider it helpful.
A common first approach we can take is running: `disassemble main` to see if we get anything useful going forwards. In this case it does, and we get the following output:
```nasm
Dump of assembler code for function main:                                                                          
   0x0000000000001398 <+0>:     endbr64                                                                            
   0x000000000000139c <+4>:     push   %rbp                                                                        
   0x000000000000139d <+5>:     mov    %rsp,%rbp                                                                   
   0x00000000000013a0 <+8>:     sub    $0x20,%rsp                                                                  
   0x00000000000013a4 <+12>:    mov    %fs:0x28,%rax                                                               
   0x00000000000013ad <+21>:    mov    %rax,-0x8(%rbp)                                                             
   0x00000000000013b1 <+25>:    xor    %eax,%eax                                                                   
   0x00000000000013b3 <+27>:    lea    0xc5e(%rip),%rax        # 0x2018                                            
   0x00000000000013ba <+34>:    mov    %rax,-0x10(%rbp)                                                            
   0x00000000000013be <+38>:    movl   $0x0,-0x14(%rbp)                                                            
   0x00000000000013c5 <+45>:    lea    -0x14(%rbp),%rax                                                            
   0x00000000000013c9 <+49>:    mov    %rax,%rdi                                                                   
   0x00000000000013cc <+52>:    call   0x132e <adjustCounter>                                                      
   0x00000000000013d1 <+57>:    mov    -0x14(%rbp),%eax
   0x00000000000013d4 <+60>:    mov    %eax,%edi
   0x00000000000013d6 <+62>:    call   0x1307 <getHop>
   0x00000000000013db <+67>:    mov    %rax,%rdx
   0x00000000000013de <+70>:    mov    -0x14(%rbp),%eax
   0x00000000000013e1 <+73>:    mov    %eax,%esi
   0x00000000000013e3 <+75>:    lea    0xc4e(%rip),%rax        # 0x2038
   0x00000000000013ea <+82>:    mov    %rax,%rdi
   0x00000000000013ed <+85>:    mov    $0x0,%eax
   0x00000000000013f2 <+90>:    call   0x10b0 <printf@plt>
   0x00000000000013f7 <+95>:    mov    -0x10(%rbp),%rax
   0x00000000000013fb <+99>:    mov    %rax,%rdi
   0x00000000000013fe <+102>:   call   0x1352 <huuu>
   0x0000000000001403 <+107>:   mov    -0x14(%rbp),%eax
   0x0000000000001406 <+110>:   add    $0x1,%eax
   0x0000000000001409 <+113>:   mov    %eax,-0x14(%rbp)
   0x000000000000140c <+116>:   mov    $0x1,%edi
   0x0000000000001411 <+121>:   call   0x10f0 <sleep@plt>
   0x0000000000001416 <+126>:   jmp    0x13c5 <main+45>
End of assembler dump.
```
We see 2 functions outside standard c functions, these being `getHop` and `huuu`. 
Looking at the `getHop` function doesn't give us a lot to work with, but disassembling `huuu` we get the following:
```nasm
Dump of assembler code for function huuu:
   0x0000555555555352 <+0>:     endbr64
   0x0000555555555356 <+4>:     push   %rbp
   0x0000555555555357 <+5>:     mov    %rsp,%rbp
   0x000055555555535a <+8>:     sub    $0x20,%rsp
   0x000055555555535e <+12>:    mov    %rdi,-0x18(%rbp)
   0x0000555555555362 <+16>:    mov    -0x18(%rbp),%rax
   0x0000555555555366 <+20>:    mov    $0xfe,%esi
   0x000055555555536b <+25>:    mov    %rax,%rdi
   0x000055555555536e <+28>:    call   0x5555555551e9 <load_s_mem>
   0x0000555555555373 <+33>:    mov    %rax,-0x8(%rbp)
   0x0000555555555377 <+37>:    mov    -0x8(%rbp),%rax
   0x000055555555537b <+41>:    mov    %rax,%rdi
   0x000055555555537e <+44>:    call   0x5555555552d8 <get_str_size>
   0x0000555555555383 <+49>:    mov    %rax,%rdx
   0x0000555555555386 <+52>:    mov    -0x8(%rbp),%rax
   0x000055555555538a <+56>:    mov    %rdx,%rsi
   0x000055555555538d <+59>:    mov    %rax,%rdi
   0x0000555555555390 <+62>:    call   0x555555555289 <unload_mem>
   0x0000555555555395 <+67>:    nop
   0x0000555555555396 <+68>:    leave
   0x0000555555555397 <+69>:    ret
End of assembler dump.
```
as we can see at the address `0x000055555555536e`, we see a call to a function named `load_s_mem`. 
After that, we can see something moved from the stack into the `rax` register after the load_s_mem function.

If we recall, the program said: `Wouldn't it be cool to be able to look into the memory?` 
So, if we set a breakpoint at the address after the call to `load_s_mem`, that being at the location `0x0000555555555373` and run the command `x $rax`, we get the following output:
`0x5555555596b0: "SecVal{[REDACTED]}"`

# Tools used:

 - gdb
 - [dashboard](https://github.com/cyrus-and/gdb-dashboard)