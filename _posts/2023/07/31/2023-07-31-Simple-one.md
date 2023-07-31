---
date: 2023-07-31 06:20:40 +02:00
categories: [SecurityValley, Web]
tags: [Web, CTF, SecurityValley]
---
This is a writeup of the web challenge Simpple one from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 1, Score: 5
# Premise
Our administrators have forgotten a file on our web server. This file is very important for us. Unfortunately, our admin also forgot the location of the file. Can you find this file?

**Link:** [https://pwnme.org/](https://pwnme.org/)
## Challenge links:
[https://pwnme.org/](https://pwnme.org/)
# Observations
We navigate to the site, and at first glance, it looks like little more than a static page. 
Viewing the source doesn't yield many results either.
Viewing the source files, we find 3 js files, `main`, `polyfills` and `runtime`, but searching for anything such as flag or SecVal give no useful results. 
The `main.js` files main purpouse seems to be rendering the site.
# Solution
Since we don't have any input fields to try our luck in, we can take another approach; attacking the web server itself.
We can start by attempting to enumerate the server with a tool like [dirbuster](https://www.kali.org/tools/dirbuster/).
Running this tool with the built in kali wordlist located at `/usr/share/wordlists/dirb/common.txt` and the following settings:
![Dirbuster Settings](/assets/images/dirb_simple_one.png).
After running for a while, we find one file that stands out when viewing our results, seen here:
![Dirbuster Results](/assets/images/dirb_2_simple_one.png)
This file stands out for two reasons: it has a 200 status code, and its size is indicating something different from other files found by the scanner (since most other files had a status code of 502 and size of 384 and 377).

If we navigate to our newly discovered page, we find the following contents:
![flag_page](/assets/images/page_simple_one.png)

Which gives us our flag.
# Tools used:
 - dirbuster
 - the file: `/usr/share/wordlists/dirb/common.txt` found in a default kali install