---
date: 2023-09-03 21:58:54 +02:00
categories: [DUCTF, Misc]
tags: [Misc, CTF, DUCTF]
---

This is a writeup of the misc challenge blinkybill from the DUCTF(https://play.duc.tf/) CTF
#### Level: beginner, Score: 100
# Premise
Hey hey it's Blinky Bill!
## Challenge files:
blinkybill.wav
# Observations
When litsening to the wav file, we hear what sounds like [morse code](https://www.britannica.com/topic/Morse-Code) in the background.
# Solution
Putting the wav file into Audacity, we dont get a lot to work with at a glance.

![file in Audacity](/assets/images/DUCTF/blinkybill_first_look.png)
If we change the view it as a spectrogram though, we can see what looks like morse codes.

![Spectrogram in audacity](/assets/images/DUCTF/blinkybill_spectogram.png)
Decoding these into their respective letters we get the following:

![Decoded Spectrogram](/assets/images/DUCTF/blinkybill_spectogram_decoded.png)

Giving us a string that could be a flag but doesnt seem quite right, `SRINGBACKTHETREES.

Looking at the morse code, and the decoded message, a logical deduction would be that instead of the
`. . .` or `S` that I currently have as the first character, I could change it to  `- . . .` or `B`
giving us a more reasonable string as the flag.

This gives us the flag we're looking for.

> DUCTF{BRINGBACKTHETREES}
{: .prompt-info }

# Tools used:
 - Audacity
 - Alternatively: [morsecode.world](https://morsecode.world/international/decoder/audio-decoder-expert.html) has a decoder that can be used
 - the morse code alphabet 
