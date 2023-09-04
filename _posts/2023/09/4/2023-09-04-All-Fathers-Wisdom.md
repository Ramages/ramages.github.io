---
date: 2023-09-04 21:45:16 +02:00
categories: [DUCTF, rev]
tags: [rev, CTF, DUCTF]
---
This is a writeup of the rev challenge All Father's Wisdom from the DUCTF(https://play.duc.tf/) CTF
#### Level: beginner, Score: 100
# Premise
We found this binary in the backroom, its been marked as "The All Fathers Wisdom" - See hex for further details. Not sure if its just old and hex should be text, or they mean the literal hex.

Anyway can you get this 'wisdom' out of the binary for us?
## Challenge files:
the-all-fathers-wisdom

# Observations
Looking at the file in Detect it Easy (or DiE), we find that the program has been
written in C/C++:

![Detect it easy](/assets/images/DUCTF/afw_die.png)

To proceed, we can have a look at the file in IDA.

![file in IDA](/assets/images/DUCTF/afw_ida.png)

We see a function main_print_flag, which sounds interesting, so we try to follow that link.

This leads us to a long incomprehensible list of characters:

![gibberish in IDA](/assets/images/DUCTF/afw_ida_2.png)

So another approach is required. 

# Solution
If we open the file in GDB (which I use a dashboard overlay made by cyrus-and in), we can try to disassemble the main function, giving us the following output:

![gdb disass main](/assets/images/DUCTF/afw_gdb_1.png) 

We see the function main.main, so we set a breakpoint there, at the address `0x43fcaf`, and step into the function.

Once there, we can run the command `layout asm` to see the coming instructions.

![gdb layout asm](/assets/images/DUCTF/afw_gdb_2.png) 

As we can see, there is a call to `os.exit` at the instruction located at `0x409952`, so we can set a breakpoint at the instruction preceding that instruction: `0x409950`. 
We're also interested in the call to `main.print_flag`, so we set a breakpoint there, at the address `0x40995b`.

Once we've done this, we can continue the execution, and reach the instruction at `0x409950`

![gdb breakpoint before exit](/assets/images/DUCTF/afw_gdb_3.png)

We dont want to step forwards just yet, since we dont want to run the `os.exit` command. 
If we instead run the command `set $rip 0x409957`, we're able to have `$rip`, the instruction pointer, skip the os.exit command.

![gdb rip repoint](/assets/images/DUCTF/afw_gdb_rip.png)

This allows us to step into the main.print_flag function.

![gdb main.print_flag](/assets/images/DUCTF/afw_print_flag_but_its_the_flag.png)

Once here, we can set a breakpoint at the instruction before the program returns 

![gdb main.print_flag breakpoint](/assets/images/DUCTF/afw_print_flag_but_its_the_flag_instruction.png)

This makes the program actually print the flag as an output to us, allbeit in hex format.

![gdb main.print_flag printout](/assets/images/DUCTF/afw_hex_flag.png)

Using a tool like CyberChef, we're able to decod the flag, which can be seen here:

![CyberChef Decoded Flag](/assets/images/DUCTF/afw_flag_win.png)

Giving us our flag
> DUCTF{Od1n_1S-N0t_C}
{: .prompt-info }


# Tools used:
 - [Detect it easy (DiE)](https://github.com/horsicq/Detect-It-Easy)
 - [IDA free](https://hex-rays.com/ida-free/)
 - [gdb](https://www.sourceware.org/gdb/)
 - [cyrus-and gdb dashboard](https://github.com/cyrus-and/gdb-dashboard)
 - [CyberChef](https://gchq.github.io/CyberChef)
