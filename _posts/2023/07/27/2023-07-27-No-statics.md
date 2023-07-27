---
date: 2023-07-27 18:14:54 +02:00
categories: [SecurityValley, Reversing]
tags: [Reversing, CTF, SecurityValley]
---
This is a writeup of the reversing challenge No statics from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 1, Score: 10
# Premise
This time there is no static value inside...but you are the best. So crack it baby!

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/no_static](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/no_static)
## Challenge files:
crackme-02
# Observations
When simply attempting to run the file, we get a prompt containing the following:
```
Again, there is something inside me!
enter your passphrase, please!
```
We respond by simply typing `a` to the program here, which gives the following output:
```
try:a
 ........
mySecret
WRONG!
```
So the full output of running the program with the prompt becomes:
```
Again, there is something inside me!
enter your passphrase, please!
a
try:a
........
mySecret
WRONG!
```

# Solution
As we can see in the output of the program, we have a string above the `WRONG` string, that being: `mySecret`.

If we rerun the script, this time entering `mySecret`, we then get the following output:

```
Again, there is something inside me!
enter your passphrase, please!
mySecret
try:mySecret ........
mySecret
SecVal{[REDACTED]}
```

So by running this, we get the flag in plaintext: `SecVal{[REDACTED]}`
# Tools used:

 - A standard linux terminal 
 - deduction