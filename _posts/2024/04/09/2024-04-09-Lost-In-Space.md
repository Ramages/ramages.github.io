---
date: 2024-04-09 22:48:45 +02:00
categories: [swampCTF, OSINT]
tags: [OSINT, CTF, swampCTF]
---

This is a writeup of the OSINT challenge Lost in Space by [swampCTF](https://swampctf.com/) 
#### Points: 25
# Premise
I think OSINT challenges are stupid! If they aren't, prove it! How far away is this? Don't bother giving me the unit, there's only one that you should be using in space anyways _(use Astronomical Units)_. _The flag entry is extremely forgiving just make sure you get the integer for distance right._

## Challenge files:

![lis.webp](https://ctf.swampctf.com/files/2b6b4c8f76f2a6ffb1153e31d06b3299/lis.webp)This image

# Observations
As we can see its a satellite. Since we want to find the specific identity of the satellite, we can try throwing it into googles reverse image search.
![reverse image search](/assets/images/swampCTF/lis/revimgs.png)

Which points to it being voyager 1.
# Solution
With a simple search, we can find Jet Propulsion Laboratorys site for Voyager 1s mission status here: https://voyager.jpl.nasa.gov/mission/status/#where_are_they_now

Which gives us the following flag:
> swampCTF{162.72}
{: .prompt-tip }


# Tools and sources used:
 - [google lense](https://lens.google/)
