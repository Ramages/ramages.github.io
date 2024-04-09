---
date: 2024-04-09 23:23:23 +02:00
categories: [swampCTF, OSINT]
tags: [OSINT, CTF, swampCTF]
---
This is a writeup of the OSINT challenge Aerial Attack by [swampCTF](https://swampctf.com/) 
#### Points: 50
# Premise
Find where this photo was taken! Make sure to keep your eyes out for the hawks though!

The flag is the truncated coordinate of this location to the hundredths. For example: (xx.xx, xx.xx)

## Challenge files:

![The challenge Image](/assets/images/swampCTF/birds/HawkChall.jpg)
This image

# Observations
As stated in the challenge premise, we need to find where the image was taken.
The easiest way to do this would be if we could derive that location from the metadata of the image.
# Solution
Using a site like [fotoforensics](https://fotoforensics.com/), we can check the approximate GPS location, if the metadata for it exists, which it did in this case, and as we can see here:
![The challenge Image](/assets/images/swampCTF/birds/coords.png)

We get the coords that we need to solve the challenge, with the following flag:
> (29.65, -82.34)
{: .prompt-tip }


# Tools and sources used:
 - [fotoforensics](https://fotoforensics.com/)