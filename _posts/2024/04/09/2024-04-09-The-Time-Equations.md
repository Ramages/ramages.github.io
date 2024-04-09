---
date: 2024-04-09 22:05:57 +02:00
categories: [swampCTF, Misc]
tags: [Misc, CTF, swampCTF]
---
This is a writeup of the misc challenge The Time Equations by [swampCTF](https://swampctf.com/) 
#### Points: 154
# Premise
I have found the secrets of time travel hidden in 5 linear equations! HOWEVER, they need to be solved within 10 seconds or else the quantumn state of the universe changes and the original equations are no longer valid! Can you do it?

## Challenge files:

[linear.c](https://ctf.swampctf.com/files/b8c9b58e1d34d16e74c2a37b6c6a0ac0/linear.c)

# Observations
Opening the linear.c file, we can see that we have a function that generates a random seed and then creates a 5x5 linear equation from that seed. 
After that, we have a timer that starts, and as the description implies, are required to solve the equation in 10 seconds or less, and provide the value of the 5 variables that make up the matrix.

![runnnig the script](/assets/images/swampCTF/timeeq/timeeq_1.png)

# Solution
I cant speak for others, but for myslef, there isnt a chance in hell that I can solve that equation in 10 seconds.
However, I dont have to, I can use python and pwntools.

Using the two, I managed to create the following script a script 

```python
import numpy as np
from pwn import *

def gaussElim(a,b):
    n = len(b)
    x = np.zeros(n, float)
    # Elimination phase
    for k in range(0, n - 1):
        for i in range(k + 1, n):
                fctr = a[i, k] / a[k, k]
                for j in range(k, n):
                    a[i, j] -= fctr * a[k, j]
                b[i] = b[i] - fctr * b[k]

    #Backwards elims
    x[n - 1] = b[n - 1] / a[n - 1, n - 1]
    for i in range(n - 2, -1, -1):
        Sum = 0
        for j in range(i + 1, n):
            Sum = Sum + a[i, j] * x[j]
        x[i] = (b[i] - Sum)/a[i,i]
        
    return x

io = remote('chals.swampctf.com', 60001)

startmsg = io.recvuntil(b'\n\n')

eq1 = io.recvuntil(b'\n')
eq2 = io.recvuntil(b'\n')
eq3 = io.recvuntil(b'\n')
eq4 = io.recvuntil(b'\n')
eq5 = io.recvuntil(b'\n')

print(startmsg)

print(eq1)
print(eq2)
print(eq3)
print(eq4)
print(eq5)

nums = eq1+eq2+eq3+eq4+eq5

nums = nums.decode("utf-8").splitlines()

nums = [line[:-1] for line in nums]

nums = [line.split('=') for line in nums]

results = [int(nums[i][1]) for i in range(5)]

nums = [nums[i][0].split('+') for i in range(5)]

a1 = int(''.join(filter(str.isdigit, nums[0][0][0:5])))
a2 = int(''.join(filter(str.isdigit, nums[1][0][0:5])))
a3 = int(''.join(filter(str.isdigit, nums[2][0][0:5])))
a4 = int(''.join(filter(str.isdigit, nums[3][0][0:5])))
a5 = int(''.join(filter(str.isdigit, nums[4][0][0:5])))

b1 = int(''.join(filter(str.isdigit, nums[0][1][0:5])))
b2 = int(''.join(filter(str.isdigit, nums[1][1][0:5])))
b3 = int(''.join(filter(str.isdigit, nums[2][1][0:5])))
b4 = int(''.join(filter(str.isdigit, nums[3][1][0:5])))
b5 = int(''.join(filter(str.isdigit, nums[4][1][0:5])))

c1 = int(''.join(filter(str.isdigit, nums[0][2][0:5])))
c2 = int(''.join(filter(str.isdigit, nums[1][2][0:5])))
c3 = int(''.join(filter(str.isdigit, nums[2][2][0:5])))
c4 = int(''.join(filter(str.isdigit, nums[3][2][0:5])))
c5 = int(''.join(filter(str.isdigit, nums[4][2][0:5])))

d1 = int(''.join(filter(str.isdigit, nums[0][3][0:5])))
d2 = int(''.join(filter(str.isdigit, nums[1][3][0:5])))
d3 = int(''.join(filter(str.isdigit, nums[2][3][0:5])))
d4 = int(''.join(filter(str.isdigit, nums[3][3][0:5])))
d5 = int(''.join(filter(str.isdigit, nums[4][3][0:5])))

e1 = int(''.join(filter(str.isdigit, nums[0][4][0:5])))
e2 = int(''.join(filter(str.isdigit, nums[1][4][0:5])))
e3 = int(''.join(filter(str.isdigit, nums[2][4][0:5])))
e4 = int(''.join(filter(str.isdigit, nums[3][4][0:5])))
e5 = int(''.join(filter(str.isdigit, nums[4][4][0:5])))

x = np.array([[a1,b1,c1,d1,e1],
              [a2,b2,c2,d2,e2],
              [a3,b3,c3,d3,e3],
              [a4,b4,c4,d4,e4],
              [a5,b5,c5,d5,e5]], float)
              
print("matrix: ", x)       

y = np.array([results[0], results[1], results[2], results[3], results[4]], float).reshape((5,1))
print("res: ", y)
result_list = gaussElim(x,y)
print(result_list)
multiplier = []
for i in range(5):
    multiplier.append(result_list[i]*10)
print(multiplier)
z = [int(a.round()) for a in multiplier]

print(z)

print(io.recv())
sleep(1)
io.sendline(str(z[0]).encode())
print(io.recv())
sleep(1)
io.sendline(str(z[1]).encode())
print(io.recv())
sleep(1)
io.sendline(str(z[2]).encode())
print(io.recv())
sleep(1)
io.sendline(str(z[3]).encode())
print(io.recv())
sleep(1)
io.sendline(str(z[4]).encode())
print(io.recv())
```
Not the most elegant, but definitely functional enough for this purpouse.

Running the script gives us the following:

![runnnig the script](/assets/images/swampCTF/timeeq/run_win.png)

Which as we can see gives us the following flag:
> swampCTF{tim3_trav3l_i5nt_r3al}
{: .prompt-tip }


# Tools and sources used:
 - python3
 - [pwntools](https://docs.pwntools.com/en/stable/)
