---
date: 2025-03-07 17:47:11 +02:00
categories: [apoorvCTF, Pwn]
tags: [Pwn, CTF, apoorvCTF]
---

This is a writeup of the Pwn challenge Kogarashi Café - The Secret Blend by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 50
# Premise
Not everything on the menu is meant to be seen.

`nc chals1.apoorvctf.xyz 3003`
## Challenge files:

[secret_blend](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/BinExp/Kogarashi%20Caf%C3%A9%20-%20The%20Secret%20Blend/files/secret_blend)

# Observations
Looking at the challenge file, we have a single executable, we can check it with a few methods such as file and checksec

![file and checksec](/assets/images/apoorvCTF/cafe2/file_checksec.png)

We see that once again, we're dealing with a 32 bit executable with some settings that make our lives as attackers easier, such as the binary not being stripped, and PIE not being enabled.

Our next step here is to see if we can check what language the program was compiled in, in my case, I do this by using [Detect it Easy (DIE)](https://github.com/horsicq/Detect-It-Easy)

![DIE](/assets/images/apoorvCTF/cafe2/DIE.png)

As we can see, the program looks like it was made in C/C++. Our next step from here is to load the program into a disassembler, in my case I'm using [IDA Free](https://hex-rays.com/ida-free)

At first upon opening the program, we see the following functions that look interesting
![IDA functions](/assets/images/apoorvCTF/cafe2/IDA_open.png)

main() isnt too interesting this time either, it prints a welcome message and calls the vuln() function
![main functions](/assets/images/apoorvCTF/cafe2/IDA_main.png)

Looking at the vuln function, we can see that the flag is loaded into memory, so if we can retrieve it from there somehow, we'll have our flag. Luckily for us, the printf function in
this program doesnt have a format string, and as such, we can try exploiting it with characters such as `%x`, `%s` and `%p`
![order_coffee function](/assets/images/apoorvCTF/cafe2/IDA_vuln.png)

# Solution
Optimally, this challenge can be tackled in a more refined way, but it can also be tackled in a more cumbersome manual way, which is the approach I took.
Starting off, as mentioned, we can spam some of printfs format specifiers to it, in this case, I went with `%p`. Doing it gave the following result:
![getting stackcontent](/assets/images/apoorvCTF/cafe2/getting_stackcontent.png)

If we load these hexcodes into a decoder like [CyberChef](https://gchq.github.io/CyberChef), the output gives us the following result:
![CyberChef decoding](/assets/images/apoorvCTF/cafe2/cyberchef.png)

as we can see here, we have what looks like the flag, allbeit a bit scrambled: `oopatcvrhT{f3M_3L_unsk43r0M_hT_eI_nahS_tdlu0`

However, if we split it up every 4th character, we'll start seeing a pattern: `oopa tcvr hT{f 3M_3 L_un sk43 r0M_ hT_e I_na hS_t dlu0`.
If we reverse each string of 4 characters here, we get the following result: `apoo rvct f{Th 3_M3 nu_L 34ks _M0r e_Th an_I t_Sh 0uld`

Which gives us the flag:
> apoorvctf{Th3_M3nu_L34ks_M0re_Than_It_Sh0uld}
{: .prompt-tip }

# Tools and sources used:
- [IDA Free](https://hex-rays.com/ida-free)
- [Detect It Easy](https://github.com/horsicq/Detect-It-Easy)
- [checksec](https://github.com/slimm609/checksec)
- [CyberChef](https://gchq.github.io/CyberChef)
