---
date: 2023-07-25 22:11:28 +02:00
categories: [SecurityValley, Coding]
tags: [Coding, CTF, SecurityValley]
---

---
date: 2023-07-25 22:11:28 +02:00
categories: [SecurityValley, Coding]
tags: [Coding, CTF, SecurityValley]
---
This is a writeup of the coding challenge Good authentication from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise

After your first strike, the development team has increased the power of there login function. Are you strong enough to break it again?

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/coding/good_authentication](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/coding/good_authentication)

## Challenge code:
```javascript
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
});

readline.question('Please enter password \n', password => {
    console.log(`Gonna check if ${password} is correct`);
    readline.close();
    validate(password)
});

function validate(password) {

    if (password.length != 12) {
        throw new Error("pass violation. wrong password length");
    }


    const block1 = Array.from(password).slice(0, 4)
    const block2 = Array.from(password).slice(4, 8)
    const block3 = Array.from(password).slice(8, 12)

    const block = [
        block1,
        block2,
        block3
    ]

    let crafted = "";

    for (let i = 0; i < block.length; i++) {
        for (let a = 0; a < block[i].length; a++) {
            if (i == 0) {
                crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 7)
            } else if (i == 1) {
                crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 11)
            } else {
                crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 9)
            }
        }
    }

    if(crafted !== "sontTbxTjffe") {
        throw new Error("pass violation. wrong credentials");
    }


    banner(password);
}

function banner(payload) {
    console.info("that was great !!!");
    console.info("run the following command to get the flag.")
    console.info(`curl -X POST http://ctf.securityvalley.org:7777/api/v1/validate -H 'Content-Type: application/json' -d '{"pass": "${payload}"}'`)
}
```
# Solution
When viewing the code, we can see the line: 
```javascript
if (password.length != 12) {
    throw new Error("pass violation. wrong password length");
}
```
meaning that we need to supply a password of 12 characters. Furthermore, from the following lines:
```javascript
const block1 = Array.from(password).slice(0, 4)
const block2 = Array.from(password).slice(4, 8)
const block3 = Array.from(password).slice(8, 12)

const block = [
	block1,
	block2,
	block3
]

let crafted = "";
```
We see 3 arrays of characters being created from the password string in iterations of 4 characters each. These 3 arrays are then put in an array of their own, the `block` variable. Finally we get an empty string which will be used later. Moving forwards, we look at the next few lines:
```javascript
for (let i = 0; i < block.length; i++) {
	for (let a = 0; a < block[i].length; a++) {
		if (i == 0) {
			crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 7)
		} else if (i == 1) {
			crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 11)
		} else {
			crafted += String.fromCharCode(String(block[i][a]).charCodeAt(0) ^ 9)
		}
	}
}

if(crafted !== "sontTbxTjffe") {
	throw new Error("pass violation. wrong credentials");
}
```
Here we see that we get our 3 character arrays being XORed with the keys 7, 11 and 9, as they're being also being added to the `crafted` string. We also get to see that the final string should be `"sontTbxTjffe"`.
If we simply split this string into 3 strings, with 4 characters in each, just like the encoding for loop did, we get the 3 strings `sont`, `TbxT`, and `jffe`. We can then XOR them with their respective keys, here I'm using [CyberChef](https://gchq.github.io/CyberChef) again. We XOR our 3 strings with the keys 7, 11 and 9 respectively, and piece them back together to get the pass.
Just like in the `Easy authentication` challenge, we finish by sending a POST request using the command:
```bash
curl -X POST http://ctf.securityvalley.org:7777/api/v1/validate -H 'Content-Type: application/json' -d '{"pass": "[redacted]"}'
```
Which returns: `{"Value":"SecVal{[redacted]}"}`, giving us the flag.