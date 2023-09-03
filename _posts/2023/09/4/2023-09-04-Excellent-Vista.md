---
date: 2023-09-04 00:11:52 +02:00
categories: [DUCTF, OSINT]
tags: [OSINT, CTF, DUCTF]
---
This is a writeup of the OSINT challenge Excellent Vista! from the DUCTF(https://play.duc.tf/) CTF
#### Level: beginner, Score: 100
# Premise
What a nice spot to stop,lookout and watch time go by, EXAMINE the image and discover where this was taken.
## Challenge files:
ExcellentVista.jpg
# Observations
We're given a jpg file. This file contains geographic coordinates
# Solution
We can use any exif tool to get the exact coordinates and any map that can resolve said coordinates to find the location we're looking for. 

In my case Im using https://tool.geoimgr.com/

Using the tool we get the following result.

![image in geoimgr](/assets/images/DUCTF/excellentvista.png)

Showing us that the picture has been taken at Durrangan Lookout

Giving us our flag
> DUCTF{Durrangan_Lookout}
{: .prompt-info }

# Tools used:
 - Any EXIF viewer
 - Any map that can resolve coordinates
 - https://tool.geoimgr.com/ 
