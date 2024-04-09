---
date: 2024-04-09 23:41:54 +02:00
categories: [swampCTF, OSINT]
tags: [OSINT, CTF, swampCTF]
---
This is a writeup of the OSINT challenge Hidden Snack Stop by [swampCTF](https://swampctf.com/) 
#### Points: 75
# Premise

I found this really good chips place, but I don't want it to be crowded, so I've blurred everything. Hahaha!

The flag is the address of this location as it's shown on google maps. Good luck!

Do not wrap the address in swampCTF{}

## Challenge files:

![The challenge Image](/assets/images/swampCTF/snackstop/Censored_Snack_Spot.png)
This image

# Observations
After a lot of loose ends and red herrings (mostly created by myself), a good point of interest was the following part of the image:

![Interesting part](/assets/images/swampCTF/snackstop/purpleX.png)
# Solution
Searching mountain Xpress, gives us a result for Ashville, North Carolina, so our search brings us there.

![Ashville](/assets/images/swampCTF/snackstop/ashville.png)

Opening google maps and remembering what we're actually looking for, a chip shop, brings us to search for one in Ashville, which gives these results:
![Google maps](/assets/images/swampCTF/snackstop/gmaps.png)
Out of the 2, the one selected in the image looks much closer to what we're actually looking for, gives us the following:
![Google street view](/assets/images/swampCTF/snackstop/streetview.png)
so we've found the place.

Now, by clicking the address, we get ```43 1/2 Broadway St, Asheville, NC 28801, United States```
which  we need to solve the challenge, with the following flag:
> 43 1/2 Broadway St, Asheville, NC 28801, United States
{: .prompt-tip }


# Tools and sources used:
 - Any search engine
 - [Google maps](https://www.google.com/maps)
