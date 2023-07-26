---
date: 2023-07-26 22:19:54 +02:00
categories: [SecurityValley, Crypto]
tags: [Crypto, CTF, SecurityValley]
---
This is a writeup of the crypto challenge Thanks Microsoft from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise
During our hack we have stolen a pieces of weird and old C code. But it seems that this was only the encoder...please reverse it and reveal the secret message

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/thanks_microsoft](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/crypto/thanks_microsoft)

## Challenge code:
encoder.c
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

u_int32_t encodeChar(unsigned char *s) {

    u_int32_t result = 0;

    unsigned int ln = (int) s & 0x0F;
    unsigned int hn = (int) s >> 4 & 0x0F;

    u_int8_t x = 0;
    u_int8_t y = 0;

    if(hn <= 9) {
        x = 11;
    } else {
        x = 26;
    }

    if(ln <= 9) {
        y = 14;
    } else {
        y = 26;
    }

    if(hn > 9) {
        hn -= 9;
    }

    if(ln > 9) {
        ln -= 9;
    }


    result |= 0x0E;
    result = result << 6;

    result |= y;
    result = result << 4;

    result |= ln;
    result = result << 6;

    result |= x;
    result = result << 4;

    result |= hn;

    return result;
}

int main(int argc, char *argv[]) {

    if(argc != 2) {
        printf("please give one text input to encode \n");
        exit(0);
    }

    char *input = malloc(strlen(argv[1]));
    strcpy(input, argv[1]);

    for(int i = 0; i < strlen(input); i ++) {
        int chr = encodeChar(input[i]);
        printf("%x", chr);
    }

    printf("\n");
    free(input);

    return 0;
}
```

## Challenge ciphertext:
```
e38cb5e394b6e38cb6e398b5e384b6e68cb6e688b7e68cb6e698b6e68cb6e698b5e390b7e3a0b6e384b4e390b7e698b5e39cb7e384b6e38cb7e698b5e694b6e698b6e698b5e394b6e694b6e38cb6e388b7e3a4b7e380b7e390b7e384b3e698b6e694b6e690b7
```
# Solution
We start off by analyzing the encoder function. This function takes a byte, splits it into its lower and higher 4 bits and then encodes them. If the higher bits make 9 or higher, they will become 11, otherwise they become 26. If the lower bits make 9 or higher, they will become 14, otherwise they will also become 26. Additionally, if either of them are above 9, they will be reduced by 9. After this some bitwise operations are performed and a resulting full byte is returned. 
We can utilize the encoding function to brute force our way to the flag.
If we make a dictionary of all characters, we get no result, but as we know, the flags are in the format: `SecVal{}` so we add the characters `{` and `}` . This still doesnt solve our problem, so we add another character that's likely to be found in the flag, in this case `_`. This solves our problem, and we can now create a dictionary of the encoded characters, and then use it on the encoded message. An example of this implementation would look something like:
```python
def encode_char(s):
    ln = ord(s) & 0x0F
    hn = ord(s) >> 4 & 0x0F

    result = 0

    if hn <= 9:
        x = 11 
    else:
        x = 26
    
    if ln <= 9:
        y = 14
    else:
        y = 26

    if(hn > 9): 
        hn -= 9

    if(ln > 9): 
        ln -= 9 
    
    result = result | 0x0E 
    result = result << 6
    result = result | y
    result = result << 4
    result = result | ln
    result = result << 6
    result = result | x
    result = result << 4
    result = result | hn
    
    return result

chunks = []
byte_encoding = {}
decoded = ""

alphabet = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '{', '}', '_']

encrypted_text = "e38cb5e394b6e38cb6e398b5e384b6e68cb6e688b7e68cb6e698b6e68cb6e698b5e390b7e3a0b6e384b4e390b7e698b5e39cb7e384b6e38cb7e698b5e694b6e698b6e698b5e394b6e694b6e38cb6e388b7e3a4b7e380b7e390b7e384b3e698b6e694b6e690b7"

#To convert the encrypted_test pieces into numbers we can use as keys in the dictionary
#we need them to be split into lengths of 6
n = 6

for a in alphabet:
    byte_encoding[encode_char(a)] = a

for i in range(0, len(encrypted_text), n):
    chunks.append(encrypted_text[i:i+n])

for chunk in chunks:
    #int(chunk, 16) converts the split chunk of 6 hex chars into a 8 number long int
    decoded += byte_encoding[int(chunk, 16)]

print(decoded)
```

Which then prints out the flag: `SecVal{[REDACTED]}`