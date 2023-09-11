---
date: 2023-09-11 10:37:25 +02:00
categories: [CyberHeroines, Forensics]
tags: [Forensics, CTF, CyberHeroines]
---
This is a writeup of the forensics challenge Barbara Liskov from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy/Medium, Score: 200
# Premise

[Elizabeth Jocelyn "Jake" Feinler](https://en.wikipedia.org/wiki/Elizabeth_J._Feinler) (born March 2, 1931) is an American information scientist. From 1972 until 1989 she was director of the Network Information Systems Center at the Stanford Research Institute (SRI International). Her group operated the Network Information Center (NIC) for the ARPANET as it evolved into the Defense Data Network (DDN) and the Internet. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Elizabeth_J._Feinler)

Chal: We found this PCAP but we did not know what to name it. Return the flag to this [Internet Hall of Famer](https://www.youtube.com/watch?v=idb-7Z3qk_o)

Author: [Rusheel](https://github.com/Rusheelraj)

## Challenge files:
NoName.pcap
# Observations
Looking at the traffic, we see a lot of DNS traffic preceded what appears to be char codes followed by some domain.

![traffic](/assets/images/CHCTF/Elizabeth/traffic.png)

# Solution
Taking the char codes only, we have the sequence: `143 150 143 164 146 173 143 162 63 141 164 60 162 137 60 146 137 144 60 155 141 151 156 137 156 64 155 63 137 163 171 163 164 63 155 175`.

We can attempt to decipher the string with a tool like [Cyberchef](https://gchq.github.io/CyberChef/).

At first, I tried deciphering the text using decimal, which gave me unusable gibberish, so that wasn't the right way forwards.

One point of note in the character sequence is that no character is 8 or 9, which could be indicative of the characters being encoded in the [Octal](https://en.wikipedia.org/wiki/Octal) system.

Trying to decipher the string once again, this time with octal decoding, we're able to decipher the string
![Flag image](/assets/images/CHCTF/Elizabeth/flag.png)

And find that the decoding works

Giving us our flag
> chctf{cr3at0r_0f_d0main_n4m3_syst3m}
{: .prompt-info }

# Tools used:
 - [Wireshark](https://www.wireshark.org/)
 - [CyberChef](https://gchq.github.io/CyberChef)
