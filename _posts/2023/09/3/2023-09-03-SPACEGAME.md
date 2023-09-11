---
date: 2023-09-03 19:05:35 +02:00
categories: [DUCTF, Reversing]
tags: [Reversing, CTF, DUCTF]
---

This is a writeup of the rev challenge SPACEGAME from the DUCTF(https://play.duc.tf/) CTF
#### Level: medium, Score: 182
# Premise
ALL YOUR BASE ARE BELONG TO US. YOU ARE ON THE WAY TO DESTRUCTION.
## Challenge files:
SPACEGAME.zip containing:
* distrlib (folder)
* * SPACEGAME.exe containing:
* * * aud (folder) with the files: boom.wav hit.wav and bowsers-castle-sincx.ogg
* * * img (folder) containing boss.png, bullet.png, player.png and playerbullet.png
* * * Boss.lua
* * * Bullet.lua
* * * conf.lua
* * * constants.lua
* * * main.lua
* * * mathutil.lua
* * * Player.lua
* * * PlayerBullet.lua
* * license.txt 
* * love.dll
* * lua51.dll
* * mpg123.dll
* * msvcp120.dll
* * msvcr120.dll
* * OpenAL32.dll
* * SDL2.dll

# Observations
In this challenge we have a few files, but the most notable one is the actual game, contained within SPACEGAME.exe.
Running the game, we have what appears to be a [bullet hell](https://www.giantbomb.com/bullet-hell/3015-321/) game where we play as a smaller spaceship fighting a larger one, and it seems the objective of the challenge is to beat the game.
# Solution
In the [official writeup](https://github.com/DownUnderCTF/Challenges_2023_Public/blob/main/rev/spacegame/solve/solve.md) of the challenge, we're expected to edit the Player.lua file to override the kill function of the player.

This is however not the only method, as another alternative will be provided here.

Using the tool beloved by any flash game cheater/hacker: [Cheat Engine](https://www.cheatengine.org/downloads.php), we can see if we can cheat in the actual game rather than by changing the coding of it.

By attaching the game process to cheat engine, we can scan the game with it with the option box `"Pause the game while scanning"` checked. 

![Cheat engine configuration](/assets/images/DUCTF/SPACEGAME_cheat_eng.png)

If we then run the scan, we see that we're able to build up a "bullet stream" that attacks the enemy ship very fast, allowing us to kill it much easier than playing the conventional way.

![Bullet Stream](/assets/images/DUCTF/SPACEGAME_bullet_beam.png)

After a successful attempt of this, we manage to destroy the enemy ship and get the following printout:

![Game win](/assets/images/DUCTF/SPACEGAME_win.png)

Giving us our flag:
> DUCTF{your_journey_is_over_a1eb723d}
{: .prompt-info }

# Tools used:
 - [Cheat engine 7.5](https://www.cheatengine.org/downloads.php)
 - any screenshot tool
