---
date: 2023-09-11 15:02:51 +02:00
categories: [CyberHeroines, Reversing]
tags: [Reversing, CTF, CyberHeroines]
---
This is a writeup of the reversing challenge Dorothy Vaughan from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy, Score: 100
[Dorothy Jean Johnson Vaughan](https://en.wikipedia.org/wiki/Dorothy_Vaughan) (September 20, 1910 – November 10, 2008) was an American mathematician and human computer who worked for the National Advisory Committee for Aeronautics (NACA), and NASA, at Langley Research Center in Hampton, Virginia. In 1949, she became acting supervisor of the West Area Computers, the first African-American woman to receive a promotion and supervise a group of staff at the center. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Dorothy_Vaughan)

Chal: We ran this Fortran Software and received the output `Final Array:bcboe{g4cy:ixa8b|m:8}`. We have no idea what this means but return the flag to the [Human Computer](https://www.youtube.com/watch?v=zMAFPRgsCDM)

Author: [Kourtnee](https://github.com/kourtnee)

## Challenge files:

dorothy (ELF executable)
    (which when ran requires a flag.txt)

# Observations
When we run the program at first, we're given an error message since the program cant find the file flag.txt

![error no flag.txt](/assets/images/CHCTF/Dorothy/1run.png)

We create an empty flag.txt file and get the following error.

![error empty flag.txt](/assets/images/CHCTF/Dorothy/emptyflag.png)

# Solution
Looking back at the description, we have a string thats given as the final output of the program: `bcboe{g4cy:ixa8b|m:8}`

If we remember the flag format of this CTF: `chctf{fl4g_h3rE}`, we can make an assumption. Since the output inside the curly
brackets is 14 characters long, we can try setting the flag.txt file to `chctf{aaaaaaaaaaaaaa}`. 

This gives us the following output:

![1st flag rev](/assets/images/CHCTF/Dorothy/1strevflag.png)

as we can see, we get the first 5 characters of the output from the description, `bcboe{``.

From this, we can deduct that we can work our way to the flag by changing the `aaaaaaaaaaaaaa` characters untill we end up with the
same output as the challenge description: `g4cy:ixa8b|m:8`

We want to reach the following output:

![output to get](/assets/images/CHCTF/Dorothy/final_arr.png)

Which we do once we've fully reversed what the content of our flag.txt file should be, which ends up containing the following:

![flagtxt](/assets/images/CHCTF/Dorothy/flag.png)

This gives us our flag
> chctf{h1dd3n_f1gur35}
{: .prompt-info }

# Tools used:
 - any text editor
 - OS to run ELF binaries