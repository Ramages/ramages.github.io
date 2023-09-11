---
date: 2023-09-11 10:34:42 +02:00
categories: [CyberHeroines, Forensics]
tags: [Forensics, CTF, CyberHeroines]
---
This is a writeup of the forensics challenge Barbara Liskov from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy, Score: 100
# Premise

[Barbara Liskov](https://en.wikipedia.org/wiki/Barbara_Liskov) (born November 7, 1939 as Barbara Jane Huberman) is an American computer scientist who has made pioneering contributions to programming languages and distributed computing. Her notable work includes the development of the Liskov substitution principle which describes the fundamental nature of data abstraction, and is used in type theory (see subtyping) and in object-oriented programming (see inheritance). Her work was recognized with the 2008 Turing Award, the highest distinction in computer science. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Barbara_Liskov)

Chal: Return the flag back to the [2008 Turing Award Winner](https://www.youtube.com/watch?v=_jTc1BTFdIo)

Author: [Josh](https://github.com/JoshuaHartz)
## Challenge files:
BarbaraLiskov.pyc
# Observations
Looking at the file given, we have a [Python Cache](https://peps.python.org/pep-3147/) file, which we can look through to see if we can find any clues that would help us solve the challenge.
# Solution
Looking through the file, we come across the following string:

![B64](/assets/images/CHCTF/Barbara/b64.png)

Which looks a lot like a base64 string.

Using a tool like [Cyberchef](https://gchq.github.io/CyberChef/), we're able to decipher the string
![Flag image](/assets/images/CHCTF/Barbara/flag.png)

And find that the decoding works

Giving us our flag
> chctf{u_n3v3r_n33d_0pt1m4l_p3rf0rm4nc3,_u_n33d_g00d-3n0ugh_p3rf0rm4nc3}
{: .prompt-info }

# Tools used:
 - Any text editor
 - [Cyberchef](https://gchq.github.io/CyberChef/)
