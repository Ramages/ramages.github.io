---
date: 2023-07-27 23:11:00 +02:00
categories: [SecurityValley, misc]
tags: [Misc, CTF, SecurityValley]
---
This is a writeup of the miscellaneous challenge The picture from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 1, Score: 5
# Premise
WTF... we need a forensic specialist here

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/miscellaneous/the_picture](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/miscellaneous/the_picture)
## Challenge files:
challenge.png
# Observations
Simply looking at the image, we see a yellow image with 2 strings: `d lab_` and `capture the flag`, and a black squiggly line.

There are a few techniques that can be attempted here, some of which can be found [here](https://ctf-wiki.mahaloz.re/misc/picture/png/), but sadly they yield little to no results.
Another tool we can use is [stegsolve](https://wiki.bi0s.in/steganography/stegsolve/), but again, this tool doesn't help us get further.
# Solution
Another tool we can use is [zteg](https://github.com/zed-0xff/zsteg) made by [zed-0xff](https://github.com/zed-0xff).
When we use this tool we get the following output:
```
b1,r,lsb,xy         .. text: "vwpR&?|"
b1,rgb,lsb,xy       .. text: "SecVal{[REDACTED]}"
b1,rgba,msb,xy      .. file: OpenPGP Public Key
b1,abgr,msb,xy      .. file: OpenPGP Secret Key
b2,r,msb,xy         .. text: "_UUUUUUUUUUUUUUUUUUUUUU"
b2,g,msb,xy         .. text: "PUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUUU"
b2,rgba,lsb,xy      .. text: ["#" repeated 110 times]
b2,abgr,lsb,xy      .. file: OpenPGP Public Key
b2,abgr,msb,xy      .. text: ["C" repeated 92 times]
b3,rgba,lsb,xy      .. text: "dtODtO`tODv"
b4,r,msb,xy         .. text: ["U" repeated 46 times]
b4,g,lsb,xy         .. file: OpenPGP Public Key
b4,g,msb,xy         .. text: ["w" repeated 183 times]
b4,b,lsb,xy         .. file: Targa image data - Map 272 x 272 x 1 +272 +4097 - 1-bit alpha
b4,b,msb,xy         .. text: ["\"" repeated 183 times]
b4,rgb,msb,xy       .. file: OpenPGP Secret Key
b4,rgba,msb,xy      .. file: OpenPGP Secret Key
```
And there's our flag.

# Tools used:
 - a bash shell
 - zsteg