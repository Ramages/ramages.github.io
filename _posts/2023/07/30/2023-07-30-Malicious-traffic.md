---
date: 2023-07-30 15:50:31 +02:00
categories: [SecurityValley, Network]
tags: [Network, CTF, SecurityValley]
---

This is a writeup of the network challenge Malicious traffic from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 2, Score: 20
# Premise
Strange things are happening here, help! Someone stole my flag. I only remember that I clicked on a file called "ICQ_PASSWORD_CRACKER.EXE", after that everything was gone... I only know that my key was "bad". Can you repeat my flag for me?

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/network/malicious_traffic](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/network/malicious_traffic)
## Challenge files:
traffic.pcapng
# Observations
Looking at the traffic in the file, we can immediately narrow things down by applying the filter !arp, since the traffic here seems uninteresting for now.
Looking at the DNS traffic, we can see a repeating pattern, that being that the traffic always end in `4530234dsf3.cdn.aws.com`.
![traffic.pcapng traffic without arp](/assets/images/mal_traffic.png)
# Solution
If we look at the contents with some text in front of it, followed by the previously mentioned `4530234dsf3.cdn.aws.com` string, we get the following:
```
0xAABgQXCRULEkwW
EA8KFgsOPgANAQUB
FGxu0xFF
```
then there's a break with a few calls to `4530234dsf3.cdn.aws.com`.
```
0xAAMQQHNAAIGSwr
ISotLCY7ICg2Jhxp
aA==0xFF
```
If we connect these strings together, we get the two strings:
```
0xAABgQXCRULEkwWEA8KFgsOPgANAQUBFGxu0xFF
```
```
0xAAMQQHNAAIGSwrISotLCY7ICg2JhxpaA==0xFF
```
Looking at the second string, we see what is likely a base64 string, albeit with some padding, that being the 0xAA and 0xFF characters, so if we remove these from both of our strings we get the following: 
```
BgQXCRULEkwWEA8KFgsOPgANAQUBFGxu
```
```
MQQHNAAIGSwrISotLCY7ICg2JhxpaA==
```
And if we now decode them from base64 in a tool like [CyberChef](https://gchq.github.io/CyberChef/), we dont get anything legible at first, but if we remember what the challenge description said, we have the key `bad` that we can use to decode the output. 
We can attempt this decoding by adding a XOR decoder. 
Since `bad` can be both a HEX and UTF-8 key, we try decoding it as HEX first (since its the default setting), but this doesnt give us any result.
If we however try changing the key to UTF-8, we get more legible output, including our flag.
![Cyberchef Result](/assets/images/mal_traf_flag.png)
# Tools used:
 - wireshark
 - CyberChef