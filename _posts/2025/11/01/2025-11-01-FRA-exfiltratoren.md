---
date: 2025-11-01 02:29:09 +02:00
categories: [FRA, Forensics]
tags: [Forensics, CTF, FRA]
---
# Exfiltratören
This is a writeup of the challenge Exfiltratören from [FRA](https://challenge.fra.se/), the Swedish National Defence Radio Establishment.

## Premise

```
Yta 0x33 AB, a secretive company within the space industry suspects that an employee is leaking sensetive information to external parties.
This is a critical situation, as there are indicators pointing to the employee being an agent for malicious actors who want to burglarize the company.
Analyze the traffic and try to find the information that has been exfiltrated.
```
#### Extra content from readme.txt
The form of the flags in the challenge is: /flagga{[[:ascii:]]+}/. Can you find more
than two flags?

Send your solution to rekrytering@fra.se. Good Luck!

## Challenge files
[Exfiltratören](https://challenge.fra.se/exfiltratoren.zip)

## Prelude
This writeup is similar in nature to my previous writeup, Cert SE,
in the sense that it handles a single file, in which several flags can be found.
In total, I recovered 4 flags. From what I gathered by looking at the challenge
file, this should be all the flags, since the traffic appears to be segmented into
4 parts. In this writeup, Ill provile you, dear reader, with a thurough explanation
of all the steps taken to find and recover these 4 flags.

## Recovered files
Within the challenge zip file, we have a pcap file named
 * exfiltratoren.pcap
This is our main file, and contains the 4 types of traffic that make up challenges.

Within the file itself, we are able to extract a few files (with some work), they are the following:
 * msg.txt
 * presentkort.pdf
 * presentkort_flag.png
 * top_secret.png

## Flag 1
Looking at the data at the top of the pcap file, we see a skull banner, and a login to a FTP server, along the sending of the file msg.txt.

![FTP server login](/assets/images/FRA/exfiltratoren/flag1_traffic.png)

We can extract the data sent to recover msg.txt, and its contents read as follows:
```
De är mig på spåren - jag går över till kamouflerad kommunikation. Vi hörs!
ZmxhZ2dhe2Z1bmN0aW9uYWxfdHJlc2hvbGRfcG93ZXJ9
```

The data on the 2nd row of the file looks encoded, so we can use a site like [dcode](https://www.dcode.fr/cipher-identifier) to identify its encoding.
The identifier gives us Base64 as the most likely candidate.

![flag1_dcode](/assets/images/FRA/exfiltratoren/flag1_dcode.png)

By using a tool like [CyberChef](https://gchq.github.io/CyberChef/), we can decode the string

![flag1_dcode](/assets/images/FRA/exfiltratoren/flag1_recovered.png)

And with this, we've found our first flag:
> flagga{functional_treshold_power}
{: .prompt-tip }


## Flag 2
For the second challenge, we see a row of several ICMP packages. After looking around with only the filter `icmp` in wireshark, nothing
in particular stands out. If we limit it to only the packages sent by the adress `10.0.0.10`, and look at the packages starting from package no 98, we can
see something interesting. Within the lower bytes of Identification found in the IPv4 package, we start seeing characters that look like they slowly spell out
a flag.

![icmp traffic](/assets/images/FRA/exfiltratoren/flag2_traffic.png)

This slowly spells out the second flag:
> flagga{exf1l_by_p1ng}
{: .prompt-tip }

## Flag 3
In the third challenge, we're dealnig with SMTP traffic. If we follow the TCP stream, we can see a long Base64 string that we could decode, again using a tool like CyberChef.

![pdf traffic](/assets/images/FRA/exfiltratoren/flag3_traffic.png)

![pdf decoded](/assets/images/FRA/exfiltratoren/flag3_b64pdf.png)

Doing so looks like we're able to decode the file mentioned in the mail: presentkort.pdf, we can save this as a .pdf file and open it, which gives us the following:

![presentkort](/assets/images/FRA/exfiltratoren/flag3_presentkort.png)

As we can see, we have a QR code along the image, if we try to read it, we get the following data:
```
7GHE+98T16D3000D00+B9US8000G20000Q00V50000T204DF-WV0002P0MB8Z98.30Y3IY+VKS0000B44J692X9W208 4%20.UGP50000%20$ETR2033695066CD10W9JN00SAE.ONFQ7000PD0BN9%Q8RA0MKH:WK000I.00C9VW83FTLN1W/669SDRU000D50 B9TB8A63WSCZ3IWZ1S8C:0238G FWXDW/YOE62VU3TFVM8KEWJE62I-N7.A%RG02T.XQTOM4/B9%842AC*V+QLGGTR/JUEWZVTE6233W99J P6P5JUQREMN8/B8%8L06*2MINEE1K:57.2C79H.SLQ9938WFGW+BW/9HH:3AAGMFK3AS4TBZ-FF40H7KAQGJ4TS/9G VXIF3DG87G+0EH0ED$N0 QH0DNZ2EDVV$A8OR7AQ8YTY7V21OQJE./PIP6VPTL$7H7FPX18L0G9PW.CFXP0 BS3G0PE/991U08%3M2R4 FYSJJRLWSGFMUBGWQ%VF5G*:AV 0X02LDWTAW6CT+WVXCQQK26K6HTJ**4%ITBJ7000-00KVE98BGPCX9E pEDZ+DQ7A5LE8QD8UC634F%6LF6E44PVDB3DPECHFEOECLFFUJC-QE3$FXOR1LU000GQ2MY85WEEECBWE-3EKH7 3EG/DPQEDLF410MD6GO7B.R0003X4MY86WEIEC*ZCXPCX C7WE510846$Q6%964W5EG6F466G7 W6GL6VK52A6646:909RI000$00KVE98B2VC7WEHH7V3EREDGDFNF6RF64W5Y96*96.SAPA7VW64G7%/67461G7P56JXJF/I000S10CY8WU8RH8+2
```

Using the cipher identifier again shows us that this most likely is Base45 data, if we decode it in CyberChef, we can see a PNG header, and we can keep using CyberChef to render the image
from this raw PNG data, which give us the following:

![flag3 flag](/assets/images/FRA/exfiltratoren/flag3_flag.png)

So as we can see in the (smaller) image, we have our third flag:
> flagga{qrazy_basy}
{: .prompt-tip }


## Flag 4

For the fourth challenge, we have a lot of UDP traffic thats split and reassembled.

![flag4 traffic](/assets/images/FRA/exfiltratoren/flag4_traffic.png)

The traffic itself contains JSON-like data, that contains the data of a png file being sent, as we can see
in the first UDP stream, containing: `{"send":{"size":2662470,"md5":"4fccf21ce07bdce70e1030081b9ae705","fn":"top_secret.png"}}` Another convenience here is that we recieve the md5
of the file we're looking for. Another useful piece of information is the initial response sent back to to the sender: `{"transfer":{"id":3752494453,"key":"425c7548030e56735ef1c01c9182ec1f","port":31337}}`. The key here will be useful later. Something else thats useful is the responses found from the reciever, that contains data in the following format: `{"chunk":{"id":3752494453,"ix":1416442,"chk":true}}`, there's only a single datachunk where the chk variable is false, so we will omit this chunk from the data we will extract later.
Moving on to the next UDP stream, we can see chunks that contain the actual data being sent, in hex format. We also have the ix key, which can be assumed to be the index for the chunk, since the
data is sent via UDP, the order doesnt really matter to the sender, so we have to reassemble it with this key using the following python script:

```python
import json

# Read JSON file
with open('j.json', 'r') as file:
    data = json.load(file)

chunks = [entry for entry in data if "chunk" in entry]
sorted_chunks = sorted(chunks, key=lambda x: x['chunk'].get('ix', float('inf')))

other_entries = [entry for entry in data if "chunk" not in entry]
sorted_data = other_entries + sorted_chunks

with open('UDP_trafficdata.json', 'w') as file:
    json.dump(sorted_data, file, indent=4)

print("Chunks sorted in sorted_output.json")

```

which gives us:

![flag4 sorted data](/assets/images/FRA/exfiltratoren/flag4_sorteddata.png)

If we extract the values from the data key, we can save all the hexdata to a single file, as previously mentioned, we need to remove the block that was marked as false.
Loading this file into CyberChef, we can hex decode it.

![flag4 decoded hex](/assets/images/FRA/exfiltratoren/flag4_directdecodedhex.png)

The data here is a bit nonsensical to us, even more so since we're attempting to find a PNG header. After trying some various decryption/decoding methods using the key mentioned earlier,
we find that using the key and RC4 gives the following result:

![flag4 rc4 decode](/assets/images/FRA/exfiltratoren/flag4_rc4decode.png)

With this, we've found our image, we can even verify it using  the md5 sum:

![flag4 md5sum](/assets/images/FRA/exfiltratoren/flag4_md5verify.png)

The final image is the following:

![top secret](/assets/images/FRA/exfiltratoren/top_secret.png)

If we look at the exifdata of the image, we can see the following:

![top_secret exifdata](/assets/images/FRA/exfiltratoren/flag4_exifdata.png)

Here we can see a user comment looking a lot like a Base64 encoded string.
If we decode this string using CyberChef we get the following result:

![flag4 flag](/assets/images/FRA/exfiltratoren/flag4_foundflag.png)

And with this, we have our fourth and final flag:
> flagga{play_the_5_tones}
{: .prompt-tip }

# Conclusion
The challenges offered this time around were good and fun to tackle, I particularly like them since forensics is my favourite CTF category.
I do enjoy seeing the challenges created by this agency (FRA), and will continue to tackle them as they get released, and as always I'll share
my progress with anyone reading here.

# Tools and sources used:
 - [Wireshark](https://www.wireshark.org/)
 - [dcode.fr](https://www.dcode.fr/)
 - [CyberChef](https://gchq.github.io/CyberChef/)
 - [sed](https://www.gnu.org/software/sed/manual/sed.html)
 - [awk](https://www.gnu.org/software/gawk/manual/gawk.html)
 - [Python3](https://www.python.org/)
