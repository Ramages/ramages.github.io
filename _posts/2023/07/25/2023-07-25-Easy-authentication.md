---
date: 2023-07-25 21:58:46 +02:00
categories: [SecurityValley, Coding]
tags: [Coding, CTF, SecurityValley]
---
This is a writeup of the coding challenge Easy authentication from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise

Let's start simple in this game. We have collected a piece of javascript. There is a validate function but we don't know the password... can you hack it?

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/blob/master/coding/easy_authentication](https://github.com/SecurityValley/PublicCTFChallenges/blob/master/coding/easy_authentication)

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
    const pass = [106,117,115,116,95,119,97,114,109,105,110,103,95,117,112];
    const pa = Array.from(password);

    for(let i = 0; i < pa.length; i++) {
        if(pa[i].charCodeAt(0) !== pass[i]) {
            throw new Error("pass violation. wrong credentials");
        }
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
As we can see in the source code, the full program takes an input, stores it in the `password` variable, and then passes this password to the `validate` function to check if the user submitted the correct password. The array `pass` however, in the `validate` function, contains what should be the expected password, but in a decimal format. We can use a tool like [CyberChef](https://gchq.github.io/CyberChef/) to decode this password with the `from Decimal` block, which gives us the password. We can then manually run the curl command seen in the `banner` function, with the full command being: 
```bash
curl -X POST http://ctf.securityvalley.org:7777/api/v1/validate -H 'Content-Type: application/json' -d '{"pass": "[redacted]"}'
```
The response we get is: `{"Value":"SecVal{[redacted]}"}`, giving us the flag. 