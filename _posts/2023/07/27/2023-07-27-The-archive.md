---
date: 2023-07-27 23:37:27 +02:00
categories: [SecurityValley, misc]
tags: [Misc, CTF, SecurityValley]
---
This is a writeup of the miscellaneous challenge The picture from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 1, Score: 5
# Premise
I've forgotten my archive password. Help me restore it

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/miscellaneous/the_archive](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/miscellaneous/the_archive)
## Challenge files:
archive.zip
# Observations
If we simply try to unpack the file, we get prompted for a password.

# Solution
A simple approach to brute force the password of the zip file is with a tool like [fcrackzip](https://www.kali.org/tools/fcrackzip/).
We can perform a dictionary attack against the file with the `rockyou.txt` password list.

In my case, the command looks like this: 
`fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt archive.zip`

Which prints out the output:
`PASSWORD FOUND!!!!: pw == [REDACTED]`, where I've put [REDACTED] as a placeholder for the actual output.

With this, we can now unlock the zip file, and when we do, we get a text file called `flag.txt`, in which we find our flag.

# Tools used:
 - a bash shell
 - fcrackzip
 - rockyou.txt