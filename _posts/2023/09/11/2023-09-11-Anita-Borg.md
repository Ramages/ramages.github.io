---
date: 2023-09-11 18:59:30 +02:00
categories: [CyberHeroines, Reversing]
tags: [Reversing, CTF, CyberHeroines]
---
This is a writeup of the reversing challenge Anita Borg from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy/Medium, Score: 200
[Anita Borg](https://cyberheroines.ctfd.io/challenges#Anita%20Borg-32) (January 17, 1949 – April 6, 2003) was an American computer scientist celebrated for advocating for women’s representation and professional advancement in technology. She founded the Institute for Women and Technology and the Grace Hopper Celebration of Women in Computing. - [Wikipeda Entry](https://en.wikipedia.org/wiki/Anita_Borg)

Chal: Have the vision to solve this binary and learn more about this [visionary](https://www.youtube.com/watch?v=qMfELzvXpBo)

Author: [TJ](https://www.tjoconnor.org)

## Challenge files:

fLag_TRACE (Elf Executable)

# Observations
Running the file we get the following output, prompting us for the flag. Entering a single `a` doesnt yield a lot of results.

![1st run](/assets/images/CHCTF/Anita/1run.png)

If we try disassebmling the main function, we get a wall of text.

# Solution

However, in this wall of text, if we're patient enough, we can look at the instructions untill we find something interesting like here:

![chars found](/assets/images/CHCTF/Anita/chars_found.png)

We have what appears to be some hex characters, the specific ones here are: `0x63` which resolves into `c`, `0x68` which resolves into `h`
and `0x63` which resolves into `c` once again. This gives us `chc` which looks an awful lot like the first characters of our flag format: `chctf{}`

Very noteworthy here as well is the fact that they all move their characters to the address `0x8053c20`. Knowing this address will be very useful.

We can utilize python within gdb and save the output of our disass main command to a file, in this case one named `flagtrace.log` with the command seen here:

![save main dissass](/assets/images/CHCTF/Anita/dump_disass.png)

the code in text:

```python
with open("./flagtrace.log", "w") as outfile:
  outfile.write(gdb.execute("disass main", to_string=True))
end
```

trying to cat out the contents of our `flagtrace.log` file, and grepping for our `0x8053c20` we get a better but still very messy result.

After experimenting around with filtering the output, I found a cohesive enough output with the command `cat flagtrace.log | awk '/0x8053c20/ && ! /%eax/ && ! /%edx/ && ! /%dl/'`.
The result of this can be seen here:

![flag chars](/assets/images/CHCTF/Anita/characters.png)

We've now found our characters: `0x63 0x68 0x63 0x74 0x66 0x7b 0x62 0x5f 0x41 0x5f 0x56 0x31 0x73 0x31 0x30 0x4e 0x41 0x72 0x59 0x5f 0x32 0x7d`

If we use a tool like [CyberChef](https://gchq.github.io/CyberChef) we can decode these characters, as seen here:

![flag chars](/assets/images/CHCTF/Anita/cyberchef_flag.png)

Giving us our flag
> chctf{b_A_V1s10NArY_2}
{: .prompt-info }

# Tools used:
 - [gdb](https://www.sourceware.org/gdb/)
 - [gdb dashboard from cyrus-and](https://github.com/cyrus-and/gdb-dashboard)
 - python (in gdb)
 - [awk](https://www.gnu.org/software/gawk/manual/gawk.html)
 - [CyberChef](https://gchq.github.io/CyberChef) (or another hex decoder