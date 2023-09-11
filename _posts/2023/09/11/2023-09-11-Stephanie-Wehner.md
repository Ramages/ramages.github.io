---
date: 2023-09-11 10:37:25 +02:00
categories: [CyberHeroines, Forensics]
tags: [Forensics, CTF, CyberHeroines]
---

This is a writeup of the forensics challenge Stephanie Wehner from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Medium, Score: 300
[Stephanie Dorothea Christine Wehner](https://en.wikipedia.org/wiki/Stephanie_Wehner) (born 8 May 1977 in Würzburg) is a German physicist and computer scientist. She is the Roadmap Leader of the Quantum Internet and Networked Computing initiative at QuTech, Delft University of Technology.She is also known for introducing the noisy-storage model in quantum cryptography. Wehner's research focuses mainly on quantum cryptography and quantum communications. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Stephanie_Wehner)

Chal: We had the flag in notepad but it crashed. Please return the flag to [this Quantum Cryptographer](https://www.youtube.com/watch?v=XzPi29O6DAc)

[Memory Dump](https://drive.google.com/file/d/1wFihQD4zdespZx1Bjw5fV_zANoNrJg9k/view?usp=drive_link)

Author: [Rusheel](https://github.com/Rusheelraj)

## Challenge files:
564d38b5-422f-6f97-6068-7ea242ed6857.vmem
# Observations
We're greeted by a .vmem file, which is a file that, as can be seen [here](https://communities.vmware.com/t5/VMware-Workstation-Pro/vmem-files-thrashing-my-HDD/td-p/1361985), is a file that contains the memory of a virtual machine, and exists while the machine is running or has crashed, and as mentioned in the challenge description, we have a scenario where a crash has occurred.

We also get the information that the process we want to know the contents of was the notepad process.

To analyze this file, we'll have to use a tool like [Volatility 2.6](https://www.volatilityfoundation.org/releases).

# Solution
To find the contents of the notepad process, we have to walk through a few steps.

First, we want to get some information about the image, which we do with the following command: `volatility_2.6_win64_standalone.exe -f 564d38b5-422f-6f97-6068-7ea242ed6857.vmem imageinfo` (when running volatility 2.6 on Windows 10), which gives us the following output:

![imageinfo](/assets/images/CHCTF/Stephanie/vol_1.png)

The most noteworthy line is the Suggested Profile line which ends in `(Instantiated with Win8SP1x64)`, which will become relevant shortly.

That is because its used in the next command, which we use to get the processes that were running at the point of the snapshot with the command: `volatility_2.6_win64_standalone.exe -f 564d38b5-422f-6f97-6068-7ea242ed6857.vmem --profile=Win8SP1x64 pslist`, which gives us the following output:

![process list](/assets/images/CHCTF/Stephanie/vol_2.png)

So, now we know the Process ID (PID) of notepad.exe (2452), this enables us to dump specifically that process.
We do this with the command: `volatility_2.6_win64_standalone.exe -f 564d38b5-422f-6f97-6068-7ea242ed6857.vmem --profile=Win8SP1x64 memdump --dump-dir=./ -p 2452`

This saves the contents of the process to a file named `2452.dmp`.

If we look at the strings of this file, with a tool like [Detect it easy](https://github.com/horsicq/Detect-It-Easy) (abbreviated as DIE), we can filter strings to show any string containing `chctf` which gives us the following

![fake flag](/assets/images/CHCTF/Stephanie/die_flag.png)

So probably not the flag we're looking for. (Trust me, I wasted an attempt submitting it).

We can however take note of the line with the most comprehensive string, 1159.
Finding this line in DIE, we also see the following lines in close proximity:

![github link](/assets/images/CHCTF/Stephanie/die_github.png)

Which looks interesting, so we can see what we can find if we go to that page.

Navigating to the github page, we see the following:

![github page](/assets/images/CHCTF/Stephanie/gh.png)

So of course we navigate into the `Secret` repository.
Inside the repository, we find the following:

![github repo files](/assets/images/CHCTF/Stephanie/gh_txt.png)

So, a lone text file named `A_Cyber_Heroine.txt`. Looking at the contents of the file, we find the following:

![github textfile content](/assets/images/CHCTF/Stephanie/gh_noflag.png)

So, no flag there. We do have 1 additional option here though, and that's checking the commit history.
Doing this shows us the following:
![github history](/assets/images/CHCTF/Stephanie/gh_old.png)

Great, there is another commit, navigating to this older commit gives us the following result:
![github flag](/assets/images/CHCTF/Stephanie/gh_flag.png)

Giving us our flag
> chctf{2023!@mu5f@!5y_1009}
{: .prompt-info }

# Tools used:
 - [Detect It Easy (DIE)](https://github.com/horsicq/Detect-It-Easy)
 - [Volatility 2.6](https://www.volatilityfoundation.org/releases)
 - [Github usage](https://docs.github.com/en)
