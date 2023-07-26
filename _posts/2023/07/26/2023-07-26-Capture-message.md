---
date: 2023-07-26 00:36:45 +02:00
categories: [SecurityValley, Crypto]
tags: [Crypto, CTF, SecurityValley]
---

This is a writeup of the 5 point crypto challenge Capture message from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise


We have captured a message. But what is the content??? Help us, please!

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/old_history](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/old_history)

## Challenge ciphertext:
```
Riihuhg vdb ylvlwhg hoghuob dqg. Zdlwhg shulrg duh sodbhg idplob pdq iruphg. Kh bh ergb ru pdgh rq sdlq sduw phhw. Brx rqh ghodb qru ehjlq rxu iroob dergh. Eb glvsrvhg uhsoblqj pu ph xqsdfnhg qr. Dv prrqoljkw ri pb uhvroylqj xqzloolqj.

Dsduwphqwv vlpsolflwb ru xqghuvwrrg gr lw zh. Vrqj vxfk hbhv kdg dqg rii. Uhpryhg zlqglqj dvn hasodlq gholjkw rxw ihz ehkdyhg odvwlqj. Ohwwhuv rog kdvwlob kdp vhqglqj qrw vha fkdpehu ehfdxvh suhvhqw. Rk lv lqghhg wzhqwb hqwluh iljxuh. Rffdvlrqdo glplqxwlrq dqqrxqflqj qhz qrz olwhudwxuh whuplqdwhg. Uhdoob uhjdug hafxvh rii whq sxoohg. Odgb dp urrp khdg vr odgb irxu ru hbhv dq. Kh gr ri frqvxowhg vrphwlphv frqfoxghg pu. Dq krxvhkrog ehkdylrxu li suhwhqghg. Brx fdq xvh wklvmxolxv dv iodj exw grqw irujhw wr irupdw.

Qrz lqgxojhqfh glvvlplodu iru klv wkrurxjkob kdv whuplqdwhg. Djuhhphqw riihqglqj frppdqghg pb dq. Fkdqjh zkroob vdb zkb hoghvw shulrg. Duh surmhfwlrq sxw fhoheudwhg sduwlfxodu xquhvhuyhg mrb xqvdwldeoh lwv. Lq wkhq gduh jrrg dp urvh euhg ru. Rq dp lq qhduhu vtxduh zdqwhg. 
```

# Solution
We can attempt to identify the cipher type of this challenge with a tool like [cipher-identifier](https://www.dcode.fr/cipher-identifier) from dcode.

The top results are Caesar cipher and ROT13 cipher.

One option is to use the [Caesar bruteforcer](https://www.dcode.fr/caesar-cipher) from dcode, which returns a plaintext as our top result.

Among the decrypted text, we find the following line:
```He do of consulted sometimes concluded mr. An household behaviour if pretended. You can use [redacted] as flag but dont forget to format.```

Which allows us to find the flag:
 `SecVal{[redacted]}`