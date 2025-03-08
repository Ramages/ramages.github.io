---
date: 2025-03-07 23:16:37 +02:00
categories: [apoorvCTF, Forensics]
tags: [Forensics, CTF, apoorvCTF]
---

This is a writeup of the Forensics challenge Ramen lockdown by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 304
# Premise
A criminal mastermind named larry stole Chef Tataka ultimate ramen recipe and yeeted it into a password-protected zip file. Inside? A sacred file with the secret to flavor nirvana. Crack the zip, save the slurp. No pressure. 🍜💀

## Challenge files:

[recipe.zip](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/Forensics/ramen-lockdown/files/recipe.zip)

# Observations
Looking at the challenge file and as mentioned in the description, we're working with a password protected zip file. We can look Inside the zip file and find a PNG image.
There is a method to crack the keys of the password protected zip file as explained [here](https://www.elcomsoft.com/help/en/archpr/known_plaintext_attack_\(zip\).html), but essentially, if we have enough known bytes, we can crack the keys of the file.
Since the first 12 bytes of a PNG file are known to us, that being `89 50 4E 47 0D 0A 1A 0A 00 00 00 0D 49 48 44 52`, we can attempt to run a tool such as [bkcrack](https://github.com/kimci86/bkcrack) against the file.
We do this with the following command, and get the following output:

```
./bkcrack -C recipe.zip -c secret_recipe.png -x 0 89504E470D0A1A0A0000000D49484452
bkcrack 1.7.1 - 2024-12-21
[01:49:06] Z reduction using 9 bytes of known plaintext
100.0 % (9 / 9)
[01:49:06] Attack on 721680 Z values at index 6
Keys: 7cfefd6a 4aedd214 970c7187
41.3 % (297843 / 721680)
Found a solution. Stopping.
You may resume the attack with the option: --continue-attack 297843
[01:50:43] Keys
7cfefd6a 4aedd214 970c7187
```

We now have our keys, `7cfefd6a 4aedd214 970c7187`, with this we can run bkcrack again to generate a passwordless zip file:
```
./bkcrack -C recipe.zip -k 7cfefd6a 4aedd214 970c7187 -D passwordless_recipe.zip
bkcrack 1.7.1 - 2024-12-21
[01:53:24] Writing decrypted archive passwordless_recipe.zip
100.0 % (1 / 1)
```

# Solution
Unzipping the unprotected password file, we get the following image:
![win_recipe](/assets/images/apoorvCTF/ramen/secret_recipe.png)

Which gives us the flag:
> apoorvctf{w0rst_r4m3n_3v3r_ong}
{: .prompt-tip }


# Tools and sources used:
- [bkcrack](https://github.com/kimci86/bkcrack)
- [the PNG file format](https://en.wikipedia.org/wiki/PNG#Examples)
