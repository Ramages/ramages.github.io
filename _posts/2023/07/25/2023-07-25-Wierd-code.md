---
date: 2023-07-25 22:42:37 +02:00
categories: [SecurityValley, Coding]
tags: [Coding, CTF, SecurityValley]
---
This is a writeup of the coding challenge Weird code from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise

I've found a weird piece of code in the internet. That looks strange....but there is a flag inside. I belive that! Help me, plz.

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/coding/weird_code](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/coding/weird_code)

## Challenge code:
```
##################################################
 15           0 LOAD_GLOBAL              0 (print)
              2 LOAD_CONST               1 ('loading application')
              4 CALL_FUNCTION            1
              6 POP_TOP

 17           8 LOAD_GLOBAL              1 (magic)
             10 LOAD_CONST               2 ('8934')
             12 LOAD_GLOBAL              2 (get_flag)
             14 CALL_FUNCTION            0
             16 CALL_FUNCTION            2
             18 STORE_FAST               0 (d)

 19          20 LOAD_GLOBAL              0 (print)
             22 LOAD_FAST                0 (d)
             24 CALL_FUNCTION            1
             26 POP_TOP
             28 LOAD_CONST               0 (None)
             30 RETURN_VALUE
None
##################################################
  4           0 LOAD_CONST               1 ('k\\PbYUHDAM[[VJlVAMVk[VWQE')
              2 RETURN_VALUE
None
##################################################
  7           0 LOAD_CONST               1 (b'')
              2 STORE_FAST               2 (out)

  9           4 LOAD_GLOBAL              0 (range)
              6 LOAD_GLOBAL              1 (len)
              8 LOAD_FAST                1 (f)
             10 CALL_FUNCTION            1
             12 CALL_FUNCTION            1
             14 GET_ITER
        >>   16 FOR_ITER                46 (to 64)
             18 STORE_FAST               3 (i)

 10          20 LOAD_FAST                2 (out)
             22 LOAD_GLOBAL              2 (bytes)
             24 LOAD_GLOBAL              3 (ord)
             26 LOAD_FAST                1 (f)
             28 LOAD_FAST                3 (i)
             30 BINARY_SUBSCR
             32 CALL_FUNCTION            1
             34 LOAD_GLOBAL              3 (ord)
             36 LOAD_FAST                0 (k)
             38 LOAD_FAST                3 (i)
             40 LOAD_GLOBAL              1 (len)
             42 LOAD_FAST                0 (k)
             44 CALL_FUNCTION            1
             46 BINARY_MODULO
             48 BINARY_SUBSCR
             50 CALL_FUNCTION            1
             52 BINARY_XOR
             54 BUILD_LIST               1
             56 CALL_FUNCTION            1
             58 INPLACE_ADD
             60 STORE_FAST               2 (out)
             62 JUMP_ABSOLUTE           16

 12     >>   64 LOAD_FAST                2 (out)
             66 RETURN_VALUE
None
```
# Solution
Here we have to do a little bit of reverse engineering. As we can see, we have what looks like some assembled form of python code. There are a few things of interest here. For starters, the line `1 ('k\\PbYUHDAM[[VJlVAMVk[VWQE')` contains what looks like an encoded message. Furthermore, we have what looks like a key via the `'8934'` line.

We can also see what looks like a for loop from the lines
```
  9           4 LOAD_GLOBAL              0 (range)
              6 LOAD_GLOBAL              1 (len)
              8 LOAD_FAST                1 (f)
```
furthermore, we have what looks like a traversal of each character in f which have been changed first to their decimal format via ord, then into byte form via bytes.
```
 10          20 LOAD_FAST                2 (out)
             22 LOAD_GLOBAL              2 (bytes)
             24 LOAD_GLOBAL              3 (ord)
             26 LOAD_FAST                1 (f)
             28 LOAD_FAST                3 (i)
```
Lastly, here we see another variable k, which uses both i and the length of k
```
             34 LOAD_GLOBAL              3 (ord)
             36 LOAD_FAST                0 (k)
             38 LOAD_FAST                3 (i)
             40 LOAD_GLOBAL              1 (len)
             42 LOAD_FAST                0 (k)
             44 CALL_FUNCTION            1
```
We also see XOR and Modulo being utilized after the previous code block thanks to the lines:
```
             46 BINARY_MODULO
             48 BINARY_SUBSCR
             50 CALL_FUNCTION            1
             52 BINARY_XOR
```
Now with the knowledge gathered from the previous code blocks, we can attempt to piece them together into something comprehensive. An example of this would be something like:
```python
f = 'k\\PbYUHDAM[[VJlVAMVk[VWQE'

k = '8934'

out = b''

for i in range(len(f)):
    out += bytes([ord(f[i]) ^ ord(k[i % len(k)])])

print(out.decode("utf-8"))
```

Which when run prints out our flag:
 `SecVal{[redacted]}`

