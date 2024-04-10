---
date: 2024-04-10 23:59:29 +02:00
categories: [swampCTF, Forensics]
tags: [Forensics, CTF, swampCTF]
---
This is a writeup of the Forensics challenge New C2 Channel? by [swampCTF](https://swampctf.com/) 
#### Points: 125
# Premise
Sometimes you can exfiltrate data with more than just plain text. Can you figure out how the attacker smuggled out the flag on our network?

## Challenge files:

[playback.pcap](https://ctf.swampctf.com/files/bb76cbdb13f936c2cf98a9621d83efdd/playback.pcap)

# Observations
Upon opening the file in wireshark, I inspect the first HTTP packet, and see the following
![first character](/assets/images/swampCTF/c2/found_s.png)

Which looks a lot like a ascii art ```s``` and part of another character.
# Solution
Following the http packets with the string ```username``` slowly prints out the flag in ascii format
![flag](/assets/images/swampCTF/c2/moreletters.png)

This slowly builds the flag:
> swampCTF{w3lc0m3_70_7h3_l4nd_0f_7h3_pc4p}
{: .prompt-tip }


# Tools and sources used:
- [Wireshark](https://www.wireshark.org/)
