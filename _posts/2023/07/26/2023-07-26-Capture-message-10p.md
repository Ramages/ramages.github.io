---
date: 2023-07-26 17:33:54 +02:00
categories: [SecurityValley, Crypto]
tags: [Crypto, CTF, SecurityValley]
---
This is a writeup of the 10 point crypto challenge Capture message from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise

Hm hm hm. We have seen this before... a long time ago. But we are to stupid to crack it...help us, again! I guess the original content of this weird garbage is english. Maybe that help's you to break it! Hint: Flag content should be lowercase.

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/weird_but_old](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/weird_but_old)

## Challenge ciphertext:
```
XOVUD IXW EDBVQQVQB GF BDG ZDHP GVHDA FN WVGGVQB EP KDH WVWGDH FQ GKD EXQR XQA FN KXZVQB QFGKVQB GF AF FQUD FH GIVUD WKD KXA SDDSDA VQGF GKD EFFR KDH WVWGDH IXW HDXAVQB ELG VG KXA QF SVUGLHDW FH UFQZDHWXGVFQW VQ VG XQA IKXG VW GKD LWD FN X EFFR GKFLBKG XOVUD IVGKFLG SVUGLHDW FH UFQZDHWXGVFQWWF WKD IXW UFQWVADHVQB VQ KDH FIQ CVQA XW IDOO XW WKD UFLOA NFH GKD KFG AXP CXAD KDH NDDO ZDHP WODDSP XQA WGLSVA IKDGKDH GKD SODXWLHD FN CXRVQB X AXVWPUKXVQ IFLOA ED IFHGK GKD GHFLEOD FN BDGGVQB LS XQA SVURVQB GKD AXVWVDW IKDQ WLAADQOP X IKVGD HXEEVG IVGK SVQR DPDW HXQ UOFWD EP KDH PFL WKFLOA LWD VOVRDODIVWUXHHFOO XW NOXB SODXWD NFHCXG GKD NOXB EDNFHD WLECVGGVQBGKDHD IXW QFGKVQB WF ZDHP HDCXHRXEOD VQ GKXG QFH AVA XOVUD GKVQR VG WF ZDHP CLUK FLG FN GKD IXP GF KDXH GKD HXEEVG WXP GF VGWDON FK ADXH FK ADXH V WKXOO ED OXGD IKDQ WKD GKFLBKG VG FZDH XNGDHIXHAW VG FUULHHDA GF KDH GKXG WKD FLBKG GF KXZD IFQADHDA XG GKVW ELG XG GKD GVCD VG XOO WDDCDA YLVGD QXGLHXO ELG IKDQ GKD HXEEVG XUGLXOOP GFFR X IXGUK FLG FN VGW IXVWGUFXGSFURDG XQA OFFRDA XG VG XQA GKDQ KLHHVDA FQ XOVUD WGXHGDA GF KDH NDDG NFH VG NOXWKDA XUHFWW KDH CVQA GKXG WKD KXA QDZDH EDNFHD WDDQ X HXEEVG IVGK DVGKDH X IXVWGUFXGSFURDG FH X IXGUK GF GXRD FLG FN VG XQA ELHQVQB IVGK ULHVFWVGP WKD HXQ XUHFWW GKD NVDOA XNGDH VG XQA NFHGLQXGDOP IXW MLWG VQ GVCD GF WDD VG SFS AFIQ X OXHBD HXEEVGKFOD LQADH GKD KDABD
```

# Solution
When looking at the ciphertext, we can see occurrences of characters frequently enough to deduct that we're dealing with a substitution cipher.
An easy way to bruteforce this can be done with the help of the page [quipqiup](https://quipqiup.com/) developed by [Edwin Olson](http://april.eecs.umich.edu/people/ebolson).

When decoding with the statistics decoder, we get a readable text as our top result, and find the result to be legible. Among the text is the line:
```SHOULD USE [REDACTED] AS FLAG PLEASE FORMAT THE FLAG BEFORE SUBMITTING```

So with this, we simply submit the flag, tough in lowercase:
 `SecVal{[redacted]}`