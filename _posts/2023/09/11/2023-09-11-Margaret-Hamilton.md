---
date: 2023-09-11 09:12:25 +02:00
categories: [CyberHeroines, Forensics]
tags: [Forensics, CTF, CyberHeroines]
---
This is a writeup of the forensics challenge Margaret Hamilton from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy, Score: 100
# Premise
[Margaret Elaine Hamilton](https://en.wikipedia.org/wiki/Margaret_Hamilton_(software_engineer) (née Heafield; born August 17, 1936) is an American computer scientist, systems engineer, and business owner. She was director of the Software Engineering Division of the MIT Instrumentation Laboratory, which developed on-board flight software for NASA's Apollo program. She later founded two software companies—Higher Order Software in 1976 and Hamilton Technologies in 1986, both in Cambridge, Massachusetts. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Margaret_Hamilton_(software_engineer)

Chal: Return the flag to [NASAs first software engineer](https://www.youtube.com/watch?v=kYCZPXSVvOQ).

Author: [Rusheel](https://github.com/Rusheelraj)
## Challenge files:
Apollo-Mystery.jpg
# Observations
Upon viewing the challenge file, we see the following image:

![Original challenge image](/assets/images/CHCTF/Margaret/Apollo-Mystery.jpg)

Which when viewing doesn't lead us anywhere at first glance, so we investigate further.
Opening the file in a hex editor, we can see the following text:

![Hidden secret flag](/assets/images/CHCTF/Margaret/Margaret_yt_link.png)

Which is a link to https://www.youtube.com/shorts/y3I-mbNCC80, which is a youtube short about the namegiver of the challenge, Margaret Hamilton.

Scrolling to the bottom of the file, we get the following data:

![Bottom of hexdata](/assets/images/CHCTF/Margaret/bottom_of_hex.png)

Where [PK](https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html) indicates some form of zipped file, which coincidentally happens to be named margaret_flag.png so of course we'd like to access this file.
# Solution
Using the tool [`binwalk`](https://github.com/ReFirmLabs/binwalk) by running the command `binwalk -e Apollo-Mystery.jpg`, we get a folder with the extracted files. (the -e flag is to extract any found contents of the file)

Within the folder of extracted files, we find a single zip file. Upon unzipping the file, we're greeted with the following image:

![Flag image](/assets/images/CHCTF/Margaret/margaret_flag.png)

Giving us our flag
> chctf{i_wr1t3_code_by_h4nd}
{: .prompt-info }

Additionally, I manage to solve this challenge quick enough to achieve the following:
![1st](/assets/images/CHCTF/Margaret/firstblood.png)

# Tools used:
 - [Binwalk](https://github.com/ReFirmLabs/binwalk)
 - any photo viewer
 - any hex viewer (optional), I used [HxD](https://mh-nexus.de/en/hxd/)
