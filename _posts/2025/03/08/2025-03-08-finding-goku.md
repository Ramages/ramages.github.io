---
date: 2025-03-08 10:29:09 +02:00
categories: [apoorvCTF, Crypto]
tags: [Crypto, CTF, apoorvCTF]
---

This is a writeup of the Crypto challenge Finding Goku by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 372
# Premise
Multiple Gokus appear, but only one is real. Spot subtle differences in power levels, speech, or battle knowledge to find the true one. Beware of trickster illusions and deceptive clones trying to mislead you. Use logic, observation, and special hints to uncover the real Saiyan warrior!

`nc chals1.apoorvctf.xyz 5002`

## Challenge files:

[challenge.py](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/Cryptography/finding%20goku/files/challenge.py)

# Observations
Looking at the challenge file, we see a program that runs through a few checks with two hex strings. These strings need to meet the following criteria to give us the flag:
- They cant be identical
- They have to be valid hex data
- They need to have the characters GOKU at the start of them
- Their md5 signatures must match

If we can achieve all these criteria, we'll have a flag.

To do this, we can use a tool like [hashclashes](https://github.com/cr-marcstevens/hashclash) md5_fastcoll, which generates md5 collisions for us, and do so with some hex data that starts with the desired stirng, `GOKU`

The two hexes I generated with this method were the following:
Hex 1:
```
474f4b55000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000f25c50622e32b4a66ed61fa6c2635ad3876b36a0ae4e4ca632e27a2d081ec0eb65e503ec383f8b52e7b96c3b3f184e253c22fd51aaa3852d919ab55eb68ddf5aa6a743114a8a6ca3dddf9a199b4fd5995e275d3b07fe401ad91bf3cc587259ae8b21dee129496386c5459d0f88fa5666ec7bd4b3c2c8dbf3ac13007ce7295b7b
```

Hex 2:
```
474f4b55000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000f25c50622e32b4a66ed61fa6c2635ad3876b3620ae4e4ca632e27a2d081ec0eb65e503ec383f8b52e7b96c3b3f984e253c22fd51aaa3852d919ab5deb68ddf5aa6a743114a8a6ca3dddf9a199b4fd5995e275dbb07fe401ad91bf3cc587259ae8b21dee129496386c5459d0f887a5666ec7bd4b3c2c8dbf3ac1300fce7295b7b
```

# Solution
Sending these two hexes to the challenge instance gives us the flag

> apoorvctf{y0u_f0und_7h3_r34l_g0ku_hurr4yyy}
{: .prompt-tip }


# Tools and sources used:
- [hashclashes](https://github.com/cr-marcstevens/hashclash)
- md5sum
