---
date: 2023-09-03 18:30:54 +02:00
categories: [DUCTF, Misc]
tags: [Misc, CTF, DUCTF]
---
This is a writeup of the misc challenge Welcome to DUCTF from the [DUCTF](https://play.duc.tf/) CTF
#### Level: beginner, Score: 100
# Premise
Looking at the challenge we have the following description:
To compile our code down here, we have to write it in the traditional Australian Syntax:( Try reading bottom up!)

¡ƃɐlɟ ǝɥʇ ʇno noʎ ʇuᴉɹd ll,ʇᴉ puɐ ɹǝʇǝɹdɹǝʇuᴉ ǝɥʇ ɥƃnoɹɥʇ ʇᴉ unɹ puɐ ǝɹǝɥ ǝpoɔ sᴉɥʇ ǝʞɐʇ ʇsnJ .ƎWWIפ uɐɔ noʎ NOʞƆƎɹ I puɐ ┴∩Oq∀ʞ˥∀M ƃuᴉoפ '¡H∀N H∀Ǝ⅄ 'ɐʞʞɐ⅄ pɹɐH 'ǝʞᴉl sǝɹnʇɐǝɟ ɔᴉʇsɐʇuɐɟ ƃuᴉɹnʇɐǝℲ

.snlԀ snlԀ ǝᴉssn∀ ǝʌᴉsnlɔuᴉ ʎʇᴉuɐɟoɹd ǝɹoɯ 'ɹǝʇsɐɟ 'ɹǝʇʇǝq ǝɥʇ oʇ noʎ ǝɔnpoɹʇuᴉ I uɐɔ ʇnq ++Ɔ ɟo pɹɐǝɥ ǝʌ,no⅄
## Challenge files:
welcome_to_ductf.aplusplus

# Observations
DUCTF is an Australian CTF, and as everyone knows, everything there is upside down, so we have to flip the text to be able to read it. Doing so gives us the following:
You've heard of c++ but can ı introduce you to the better, faster, more profanᴉty inclusive Aussie plus plus˙

Featuring fantastic features ןlike, hard Yakka, YeAh nAh!, Going WAlkAbout and ı reckon you can GIMMIE˙ ɾust take this code here and run it through the interpreter and it'll print you out the flag!

Now that we have our challenge description, we can proceed with analysing the welcome_to_ductf.aplusplus file.

Inside the file, we find the following text:
```
¡***Ɔ SɹƎƎHƆ

;„¡Ⅎ┴Ɔ ǝɥʇ ɟo ʇsǝɹ ǝɥʇ ʎoɾuƎ„ ƎWWIפ

;()Ⅎ┴Ɔ_ƎH┴

<
;H┴MƎɹ┴S + ɹnoHʎddɐH + Ⅎ∀˥פ ƎWWIפ	
;„ɔoɹɔ ɐ ɹɐǝu ʇᴉ ʇɟǝl oƃuoɹp ʎpoolq ʇɥƃᴉɹ ǝɯos 'ʇᴉ punoɟ I 'ǝʇɐɯ llǝɥ ʎpoolq„ ƎWWIפ  
<	
;SIH┴ ʞƆ∩Ⅎ Ǝ┴∀W ¿ 0 == (9 '0)ǝɔᴉDǝɯoSʞɔnɥƆ NOʞƆƎɹ ∀⅄		

;(000Ɩ)ʞɔɐSǝɥ┴ʇᴉH		
		
;„...ƃɐlɟ ɐʎ sᴉ ɥɐlɐƃ ,uᴉɯɐlɟ ǝɥʇ ǝɹǝɥM„ ƎWWIפ    	
> (¡H∀N 'H∀Ǝ⅄) ˥I┴N∩ ┴∩Oq∀ʞ˥∀M ∀ ƎΛ∀H ˥˥,I NOʞƆƎɹ I	
;„ƎɹƐɥʍƐɯoϛ_ʞɔ0lƆoϛ-sʇƖ„ = ɹnoHʎddɐH NOʞƆƎɹ I    
;„¡ǝʇɐɯ ɐʎ ɹoɟ ƃɐlɟ ǝɥʇ u,ɥɔʇǝℲ„ ƎWWIפ	
> () SI Ⅎ┴Ɔ_ƎH┴ ɹOℲ ∀ʞʞ∀⅄ Dɹ∀H ƎH┴
;„{Ⅎ┴Ɔ∩D„ = Ⅎ∀˥פ NOʞƆƎɹ I

  
<
;(000ϛ)ʞɔɐSǝɥ┴ʇᴉH  
  
<  
;פ∀˥Ⅎ_∀⅄ ƎWWIפ    
> ¿ Ɩ == Qqq_ƎW NOʞƆƎɹ ∀⅄  

;„}¡ǝʇɐWǝɹǝHʇ,uᴉ∀ƃɐlℲɐ⅄{∩DℲ┴Ɔ„ = פ∀˥Ⅎ_∀⅄ NOʞƆƎɹ I  
;Ɩ = Qqq_ƎW NOʞƆƎɹ I  
  
;(000ϛ)ʞɔɐSǝɥ┴ʇᴉH  
;„פ∀˥Ⅎ ƎH┴ ┴NIɹԀ S┴Ǝ˥ '¡Ǝ┴∀W H∀Ǝ⅄„ ƎWWIפ  
> () SI פ∀˥Ⅎ_┴NIɹԀ ɹOℲ ∀ʞʞ∀⅄ Dɹ∀H ƎH┴

;ǝɔᴉDǝɯoSʞɔnɥƆ ƆN∩Ⅎ ƎW ┴HOԀWI
;„}„ = H┴MƎɹ┴S NOʞƆƎɹ I
;ʞɔɐSǝɥ┴ʇᴉH ƆN∩Ⅎ ƎW ┴HOԀWI

¡Ǝ┴∀W ⅄∀D,פ
```
Once again, we have some upside down text. We once again flip this over to get the following text:
```

G'dAY MATE!

IMPOHT ME FUNC HitTheSack;
I rECkON STrEWTH = „{„;
IMPOHT ME FUNC ChuckSomepice;

THE HArp YAkkA FOr PrINT_FLAG IS () <
  GIMME „YEAH MATE!, LETS PrINT THE FLAG„;
  HitTheSack(5000);
  
  I rECkON ME_bbQ = 1;
  I rECkON YA_FLAG = "CTFDU}YaFlagAin'tHereMate!{";

  YA rECkON ME_bbQ == 1 ? <
    GIMME YA_FLAG;
  >
  
  HitTheSack(5000);
>
  

I rECkON GLAF = „DUCTF}„;
THE HArp YAkkA FOr THE‾CTF IS () <
	GIMME „Fetch'n the flag for ya mate!„;
    I rECkON HappyHour = "1ts-5oCl0ck_5om3wh3rE";
	I rECkON I'LL HAVE A WALkAbOUT UNTIL (YEAH, NAH!) <
	    GIMME "Where the flamin' galah is ya flag˙˙˙";
		
		HitTheSack(1000);

		YA rECkON ChuckSomepice(0, 6) == 0 ? MATE FUCk THIS;
	>
  GIMME „bloody hell mate, I found it, some right bloody drongo left it near a croc„;
	GIMME GLAF + HappyHour + STrEWTH;
>

THE_CTF();

GIMME „Enjoy the rest of the CTF!„;

CHEErS C***!
```
# Solution
Looking at the flipped code, and keeping in mind the wrapper for all flags in the challenge: `DUCTF{}`, we see the variable `HappyHour` contains the string `1ts-5oCl0ck_5om3wh3rE` .

Wrapping this in the `DUCTF{}` wrapper, we get the flag.


> DUCTF{1ts-5oCl0ck_5om3wh3rE}
{: .prompt-info }

# Tools used:
 - https://www.duplichecker.com/reverse-text-generator
 - any text editor
