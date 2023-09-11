---
date: 2023-09-11 14:40:19 +02:00
categories: [CyberHeroines, Forensics]
tags: [Web, CTF, CyberHeroines]
---
This is a writeup of the web challenge Grace Hopper from the CyberHeroines(https://cyberheroines.ctfd.io/) CTF
#### Level: Easy, Score: 100
[Grace Brewster Hopper](https://en.wikipedia.org/wiki/Grace_Hopper) (née Murray; December 9, 1906 – January 1, 1992) was an American computer scientist, mathematician, and United States Navy rear admiral. One of the first programmers of the Harvard Mark I computer, she was a pioneer of computer programming who invented one of the first linkers. Hopper was the first to devise the theory of machine-independent programming languages, and the FLOW-MATIC programming language she created using this theory was later extended to create COBOL, an early high-level programming language still in use today. - [Wikipedia Entry](https://en.wikipedia.org/wiki/Grace_Hopper)

Chal: Command [this webapp](https://cyberheroines-web-srv2.chals.io/vulnerable.php) like [this Navy Real Admiral](https://www.youtube.com/watch?v=1LR6NPpFxw4)

Alternate (Better) Connection: [webapp](http://ec2-3-144-228-78.us-east-2.compute.amazonaws.com:6262/vulnerable.php)

Author: [Sandesh](https://github.com/Sandesh028)

## Challenge site:

https://cyberheroines-web-srv2.chals.io/vulnerable.php

# Observations
When opening the web page, we're greeted by a textbox prompting us to run a command.

The first command that comes to mind for me ofc is to run ls.

But as we can see, we're going to have to do a little more than that

![no ls for you](/assets/images/CHCTF/Grace/no_ls_for_you.png)

The result from the cat command is the same.

Interesting but ultimately optional to solve the challenge is checking the vulnerable.php file which has the following content:

```php
<?php $output = "cmd";
if (isset($_GET[cmd])) {
	$cmd = $_GET[cmd];
	$blacklist = [cat, ls, more, tac, nl, head, tail, awk, sed];
	$safe_to_run = true;
	foreach ($blacklist as $word) {
		if (strpos($cmd, $word) !== false) {
			$safe_to_run = false;
			$output = "You think it's that easy? Try harder!";
			break;
		}
	}
	if ($safe_to_run) {
		ob_start();
		system($cmd);
		$output = ob_get_clean();
	}
} ?>
``` 

# Solution
After searching around for a few alternatives to commands like cat and ls, I found some alternative uses to the `echo` command

If we type `echo *`, it will act more or less like ls, in the sense that it prints out all files, so for now, we can make the
parallel of `ls` = `echo *`. We can see this in practice here:

![echo star](/assets/images/CHCTF/Grace/echostar.png)

Now that we've got an ls alternative, we want to view the contents of the file.

This can be done in a few ways, but 2 examples I found were either by using `strings` or by using the `xargs echo < [FILE]` command.

Running either of these on the cyberheroines.txt file gives us the output `e2d49cb900cc2b8aad02d972099366c44381e3e7c24736312ca839fbd18743a7  -`
Which we cant interpret as it is a SHA256 string.

There is another interesting file that we saw when listing the files though, `cyberheroines.sh`.

Using either of the commands to get the filecontent gives us the following:

![shellscript_content](/assets/images/CHCTF/Grace/flag.png)

Giving us our flag
> CHCTF{t#!$_!s_T#3_w@Y}
{: .prompt-info }

# Tools used:
 - *nix shell commands