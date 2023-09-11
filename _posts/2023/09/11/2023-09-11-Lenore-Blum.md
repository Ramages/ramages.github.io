---
date: 2023-09-11 13:44:39 +02:00
categories: [CyberHeroines, Forensics]
tags: [Forensics, CTF, CyberHeroines]
---
This is a writeup of the forensics challenge Lenore Blum from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Medium, Score: 300
# Premise

[Lenore Carol Blum](https://en.wikipedia.org/wiki/Lenore_Blum) (née Epstein born December 18, 1942) is an American computer scientist and mathematician who has made contributions to the theories of real number computation, cryptography, and pseudorandom number generation. She was a distinguished career professor of computer science at Carnegie Mellon University until 2019 and is currently a professor in residence at the University of California, Berkeley. She is also known for her efforts to increase diversity in mathematics and computer science. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Lenore_Blum)

Chal: Connect to 0.cloud.chals.io 28827 and return the flag to the computational mathematics professor [from this random talk](https://www.youtube.com/watch?v=GlKyizqdGIY)

Author: [Robbie](https://github.com/Robster4911)

## Challenge files:
chal.bin
# Observations
For this challenge, I strongly reccomend loking at the [official](https://github.com/FITSEC/cyberheroines) writeup of the
challenge, since it actually explains how the challenge works and likely "should" be solved.

In the challenge, we have a numbers game, where we're given a seed and asked to predict the number that the seed will generate.

If you like finding exploits through the following method:

![Funny](/assets/images/CHCTF/Lenore/trying.png)

You've come to the right place

# Solution
What I discovered was that, as the game runs, if you spam it enough times, you'll get duplicate seeds.

Since the challenge prints out what the expected result of a seed was, we can use this to our advantage, since
we just need to keep spamming untill we find ourselves a seed appearing twice, as shown here.

![Spam Poc](/assets/images/CHCTF/Lenore/spam_poc.png)

This is done when running the file locally, so we can make an attempt to recreate this "solution" on the live insance, as seen here:

![Spam flag](/assets/images/CHCTF/Lenore/spam_flag.png)


Giving us our flag
> chctf{cr3at0r_0f_d0main_n4m3_syst3m}
{: .prompt-info }

# Tools used:
 - netcat
 - spamming
