---
date: 2023-09-04 01:08:15 +02:00
categories: [DUCTF, Misc]
tags: [Misc, CTF, DUCTF]
---
This is a writeup of the misc challenge impossible from the DUCTF(https://play.duc.tf/) CTF
#### Level: beginner, Score: 100
# Premise
Lol I thought it would be funny to commit a CTF iceberg taboo and make an unsolvable CTF challenge, then gaslight competitors in support tickets.

There is no solution. I don't even know why I published files for this challenge anyway.
## Challenge files:
impossible.zip containing a python webapp

The link: https://misc-impossible-2fb66cb8eaca.2023.ductf.dev

# Observations
When looking at the unzipped files in impossible, we see a app folder containing the contents of the 
webapp.

Within this folder we have an additional interesting folder: utils.

In utils, we find `crypto.py`, which, when looked through contains what looks like an encrypted message.
The file contains the following content:
```python
from Crypto.Protocol.KDF import PBKDF2
from Crypto.Hash import SHA512
from Crypto.Random import get_random_bytes
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import binascii

# lol u thought it would be easy that i gave you these values in the sauce code
# i have changed them all on the challenge instance just to make sure it is impossible to solve
FLAG_ENC='5700668797b137bb540ff5c7febf0f48687911940afae0d5e98ca7c80c25ab475ccdc7f4cd4c04b2890d38f0048abd88bf9d10339b7d37962b1cc0668f9d945e192286d517add90fdd39c923424e282b5f34c1067f467bcfdca1f2e819e3aa667e4452c7da74880cba6d31f10b88eb72c7e6215eb445cc9c9e9d435bf2eae1e5b1d6593f43a26193d5cb8e8745e5a8d81f26efd4aa9f47e364f529d0a334613a'
KEY='8deee6793066fa6529a9af2ef31723ca6d78da5c175d78412294604a2a62af82'
IV='104e20c2abf12b342fe27064cead0c4f'

def gen_key(passphrase: str) -> bytes:
    salt = get_random_bytes(16)
    return PBKDF2(passphrase, salt, 32, count=1000000, hmac_hash_module=SHA512)

def encrypt_print(flag: str, key: bytes):
    iv = get_random_bytes(16)
    cipher = AES.new(key, AES.MODE_CBC, iv=iv)
    ct = cipher.encrypt(pad(flag.encode(), 32))
    print(f"FLAG_ENC='{ct.hex()}'")
    print(f"KEY='{key.hex()}'")
    print(f"IV='{iv.hex()}'")

def decrypt(key_hex: str) -> str:
    if key_hex != KEY:
        return "Lol we told you this is impossible to solve!"
    key = binascii.unhexlify(key_hex)
    flag_enc = binascii.unhexlify(FLAG_ENC)
    iv = binascii.unhexlify(IV)

    cipher = AES.new(key, AES.MODE_CBC, iv=iv)
    pt = unpad(cipher.decrypt(flag_enc), 32)

    try:
        return pt.decode()
    except:
        return "Cannot decode plaintext!"
    
if __name__ == "__main__":
    flag = input("flag: ")
    passphrase = input("passphrase: ")
    key = gen_key(passphrase)
    encrypt_print(flag, key)
```

As we can see, there is a commented message saying that we wont be able to decrypt the message.
Doing this anyway with a tool like CyberChef gives us the following result.
![CyberChef decoded sourcecode msg](/assets/images/DUCTF/impossible_sourcemsg.png)

So as we can see, we need to try something different to solve the challenge.

# Solution
In the utils folder, we additionally see the folder [`__pycache__`](https://ecoagi.ai/topics/Python/pycache).
In the `__pycache__` folder, we find the file `crypto.cpython-310.pyc` which contains a cached version of the `crypto.py` script and thereby also the previously used FLAG_ENC, KEY and IV variables.

Opening the file, we find the old variables, 

![pycache file](/assets/images/DUCTF/impossible_cached.png)

If we try to decrypt the message this time with our new variables, we get the following output:

![CyberChef win](/assets/images/DUCTF/impossible_win.png)

Giving us our flag
> DUCTF{o0p5_i_f0rG0t_aBoUt_pYc4Ch3!!1!}
{: .prompt-info }

# Tools used:
 - [Cyberchef](https://gchq.github.io/CyberChef/)
 - Any text editor