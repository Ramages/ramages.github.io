---
date: 2023-07-27 00:14:25 +02:00
categories: [SecurityValley, Crypto]
tags: [Crypto, CTF, SecurityValley]
---
This is a writeup of the crypto challenge Ransomware attack from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise
Oh no! We were victims of a ransomware attack. Our forensic experts were only able to quickly secure parts of the memory. We could only find out that an IV vector of "a5bc0616852c6850a422e662db4f36f8" bytes was used by exchanging information with other victims. Can you recover our most valuable file? Please.

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/ransomware_attack](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/ransomware_attack)

## Challenge files:
flag.buttlock
evil_ransomware_page_01.page
evil_ransomware_page_02.page
evil_ransomware_page_03.page
evil_ransomware_page_04.page
evil_ransomware_page_05.page

# Solution
If we look at the files through a hex editor (in my case that's [HxD](https://mh-nexus.de/en/hxd/)), we can see that the flag.buttlock file contains the following:
```
67254DDCEBA869A494E673983653C186558A2587C4314BD64CAD5094EBE4EC2E4CCFDBFA0203916D7AD5B633C2B88EB0
```
Which doesnt help us very much.
The evil_ransomware files however might be able to help us a bit more. 

When looking at the files, the first one looks like it contains an [ELF header](https://linuxhint.com/understanding_elf_file_format/). Going forwards, we can extract the content of all files and pipe them into a single file, which we will aptly name `evil_ransomware`.

We can observe the strings of the file, and through this, deduct that the executable is written in GOlang. 

Opening the file in a reverse engineering tool like [Ghidra](https://github.com/NationalSecurityAgency/ghidra) with a plugin, like the [one](https://github.com/getCUJO/ThreatIntel/blob/master/Scripts/Ghidra/go_func.py) provided by [Dorka Patolay](https://cujo.com/reverse-engineering-go-binaries-with-ghidra/), we can run it to rename parts of the binary, and rerun the `strings` command, this time using `grep` to look for anything related to encryption. 

When using `grep` with the phrase `Encrypt` we get a few interesting results, but the most important find is that there are strings that look like references named  `NewCBCEncrypter`, (and a few others related to cbc encryption) which means that we're dealing with [CBC](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) mode AES encryption. 

We already have 2 of the 3 components we need to decrypt our `flag.buttlock` file, that being:

 - The ciphertext, which in this case is the content of the `flag.buttlock` file in hex format
 - The IV vector

So all we need now is the 3rd component, the key.

Depending on which AES we use, our key will have different sizes, `AES-128` need a key of 16 bytes, `AES-192` need a key of 24 bytes and `AES-256` require a key of 32 bytes.

To get our potential keys, we can use grep again, with its -Po flag this time.
The commands
```bash
strings evil_ransomware | grep -Po '[A-Fa-f0-9]{16,16}'
strings evil_ransomware | grep -Po '[A-Fa-f0-9]{32,32}'
strings evil_ransomware | grep -Po '[A-Fa-f0-9]{64,64}'
``` 
give us our potential keys, depending on what size we choose.

I can reveal here that we're dealing with `AES-256` so the last command is the way to go.

The output of this command is:
```
ed11368683772161602973937988281255684341886080801486968994140625
e142108547152020037174224853515625710542735760100185871124267578
ce17763568394002504646778106689453125888178419700125232338905334
e111022302462515654042363166809082031255551115123125782702118158
ed13877787807814456755295395851135253906256938893903907228377647
1734723475976807094411924481391906738281258673617379884035472059
df81970d9787afbe97cbe63d3abccdec700c58187c0906e226434f77eeca9295
d000102030405060708091011121314151617181920212223242526272829303
1323334353637383940414243444546474849505152535455565758596061626
3646566676869707172737475767778798081828384858687888990919293949
AAAAAAAAAAAAAAAAAAAAABBBBBBBBBBBCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCCC
```
Among these strings is our key, and when using a tool like [CyberChef](https://gchq.github.io/CyberChef) we can provide all inputs to a AES Decoder.

When decoded, we get the flag in plaintext: `SecVal{[REDACTED]}`