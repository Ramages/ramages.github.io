---
date: 2024-04-10 23:01:29 +02:00
categories: [swampCTF, Forensics]
tags: [Forensics, CTF, swampCTF]
---
This is a writeup of the Forensics challenge Notoriously Tricky Login Mess 1 & 2 by [swampCTF](https://swampctf.com/) 
#### Points: 100 & 231
# Preface
This challenge was split in 2 parts, but as I will explain, both sections will be solved in close conjunction and therefore I decided to smash the writeups for the 2 into a single one.

# Premise
1. We found out a user account has been compromised on our network. We took a packet capture of the time that we believe the remote login happened. Can you find out what the username of the compromised account is?
2. Great job finding the username! We want to find out the password of the account now to see how it was so easily breached. Can you help?

## Challenge files:

[swampchall.pcap](https://ctf.swampctf.com/files/f2d361a338fa0068e364427c24512373/swampchall.pcap)

# Observations
As stated in the description for challenge 1, we're looking for the username account that was compromised, and then for 2, its passwords.

Opening wireshark, we're initially greeted by the following:
![Initial wireshark](/assets/images/swampCTF/login/wsrkopen.png)

As highlighted in the image, we se a ```NTLMSSP_AUTH``` POST request being made to the ```\Administrator``` account.

# Solution
If we filter out the traffic to only show us ntlmssp traffic, 
![Initial wireshark](/assets/images/swampCTF/login/win1.png)

we see another account instead of only ```\Administrator```, namely ```\adamkadaban```, which is our 1st flag.

Now however, we're onto the 2nd challenge, retrieving the password of the account.

To do this, we want to extract the NTLMv2 hash and run it against a password cracker like hashcat.

Extracting the hash can be tedious, but I found a great explanation of how to do it [here](https://www.youtube.com/watch?v=lhhlgoMjM7o).

The focus is on extracting these 5 fields:
```
User:
Domain:
Challenge:
HMAC-MD5:
NTLMv2Response:
```
Which in our case, gives us a total of 6 for the user ```\adamkadaban```, one example of them is here:
```
User: adamkadaban
Domain: NULL
Challenge: 1fed9e8e0ca470a3
HMAC-MD5: 98ebffae0b77865893846dfadb757cfb
NTLMv2Response: 0101000000000000801c50dbc266da0188d48d08eff230a80000000002001e0045004300320041004d0041005a002d00450033003300530047004c00380001001e0045004300320041004d0041005a002d00450033003300530047004c00380004001e0045004300320041004d0041005a002d00450033003300530047004c00380003001e0045004300320041004d0041005a002d00450033003300530047004c003800070008005783ebd6c266da010000000000000000
```
With all this data, we can now run this agains a hascat instance.
First however, we need to puzzle it together into a format hashcat [understands](https://hashcat.net/wiki/doku.php?id=example_hashes), which in this case would be the following:
```adamkadaban:::1fed9e8e0ca470a3:98ebffae0b77865893846dfadb757cfb:0101000000000000801c50dbc266da0188d48d08eff230a80000000002001e0045004300320041004d0041005a002d00450033003300530047004c00380001001e0045004300320041004d0041005a002d00450033003300530047004c00380004001e0045004300320041004d0041005a002d00450033003300530047004c00380003001e0045004300320041004d0041005a002d00450033003300530047004c003800070008005783ebd6c266da010000000000000000```

If we now run the command ```hashcat -m 5600 -a0 ntlmv2.txt rockyou.txt``` where rockyou is the rockyou list found in ```/usr/share/wordlists/``` in a Kali Linux machine and ```ntlmv2.txt``` is a file containing the hash, we get the following result:

![Cracked password](/assets/images/swampCTF/login/win2.png)

And with that, we've found our second flag.

So now we have the flags:
> swampCTF{adamkadaban}
{: .prompt-tip }

and
> swampCTF{emilyyoudontknowmypassword}
{: .prompt-tip }

# Tools and sources used:
- [Wireshark](https://www.wireshark.org/)
- [Hashcat](https://hashcat.net/hashcat/)
