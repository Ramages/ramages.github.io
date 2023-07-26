---
date: 2023-07-27 00:14:25 +02:00
categories: [SecurityValley, Reversing]
tags: [Reversing, CTF, SecurityValley]
---
This is a writeup of the reversing challenge Simple ELF from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise
Can you deal with dwarfs and elfs`

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/the_elf](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/the_elf)

## Challenge files:
crackme-01
# Observations
When simply attempting to run the file, we get the following output:
```
Sometimes, there is something inside me. Can you get it?
S
Whoops, that was weird??
```
Which doesn't allow us as users to do a lot, so we move on to another method.
# Solution
If we run the strings command, simply:
```bash
strings crackme-01
```
We're able to find our flag among the strings outputted. To simplify it even further, we can run:
```bash
strings crackme-01 | grep SecVal
```
When running the second command, we get the flag in plaintext: `SecVal{[REDACTED]}`
# Tools used:

 - A standard linux terminal 
 - bash
 - grep