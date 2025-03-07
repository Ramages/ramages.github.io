---
date: 2025-03-07 22:48:40 +02:00
categories: [apoorvCTF, Forensics]
tags: [Forensics, CTF, apoorvCTF]
---

This is a writeup of the Forensics challenge Samurai’s Code by [apoorvCTF](https://apoorvctf.iiitkottayam.ac.in/) 
#### Points: 162
# Premise
Unveil the lost code of the Samurai and unlock the mystery hidden within.

## Challenge files:

[sam.jpg](https://github.com/CSYClubIIITK/CTF-Writeups/blob/main/ApoorvCTF-25-Writeups/Forensics/Samurai%E2%80%99s%20Code/files/sam.jpg)

# Observations
We start off by looking at the challenge image:
![challenge_img](/assets/images/apoorvCTF/samurai/sam.jpg)

Nothing seems out of the ordinary from the image, so we need to start investigating a bit further.
Exiftools didnt yied anything of interest, but we can continue to the site [Fotoforensics](https://fotoforensics.com/)

![fotoforensics](/assets/images/apoorvCTF/samurai/fotoforensics.png)

Here we can see what looks a lot like [Brainfuck](https://esolangs.org/wiki/Brainfuck) at the end of the image.

```Brainfuck
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>++++.++++++++++++..----.+++.<------------.-----------..>---------------.++++++++++++++.---------.+++++++++++++.-----------------.<-.>++.++++++++..--------.+++++.-------.<.>--.++++++++++++.--.<+.>-------.+++.+++.-------.<.>-.<.++.+++++++++++++++++++++++++.+++++++++++++.>+++++++++++++.<+++++++++++++.----------------------------------.++++++++.>+++++++++.-------------------.<+++++++.>+.<-----.+++++++++.------------.<+++++++++++++++.>>++++++++++++++++.<+++.++++++++.>-.<--------.---------.++++++++++++++++++++.>.<++.>--------------.<<+++++.>.>-----.+++++++.<<++.>--.<++.---------.++.>>+++++++++++.-------------.----.++++++++++++++++++.<<++++++++++++++++.>>--.--.---.<<--.>>+++.-----------.-------.+++++++++++++++++.---------.+++++.-------.
```

Running the code using [dcode.fr](https://www.dcode.fr/brainfuck-language), we get the following output:
![brainfuck_decode](/assets/images/apoorvCTF/samurai/brainfuck.png)

We get [this](https://drive.google.com/file/d/1JWqdBJzgQhLUI-xLTwLCWwYi2Ydk4W6-/view?usp=sharing) link to a google drive, navigating to it we can download a file simply titled samurai.

Opening the file, we see what looks like a jpg with its bits shuffled
![samurai_bytes](/assets/images/apoorvCTF/samurai/samurai_bytes.png)

as according to the [JPEG file format](https://en.wikipedia.org/wiki/JPEG_File_Interchange_Format#File_format_structure), we should see the first bytes be `FF D8 FF E0`

# Solution
All we need to do to recover the bit shuffled image is shuffle them back, which we can do with the following python script:

```python
def shift_bytes(file_path, output_path):
    with open(file_path, 'rb') as f:
        data = bytearray(f.read())

    for i in range(0, len(data) - 1, 2):
        data[i], data[i + 1] = data[i + 1], data[i]

    with open(output_path, 'wb') as f:
        f.write(data)

    print("Bitshifting complete")

input_file = 'samurai'
output_file = 'shifted_samurai.jpg'
shift_bytes(input_file, output_file)
```

The result is the following image:
![win_samurai](/assets/images/apoorvCTF/samurai/shifted_samurai.png)

Which gives us the flag:
> apoorvctf{ByT3s_OUT_OF_ORd3R}
{: .prompt-tip }


# Tools and sources used:
- [Fotoforensics](https://fotoforensics.com/)
- [dcode.frs brainfuck site](https://www.dcode.fr/brainfuck-language)
- A hex editor of choice
- [the JPEG file format](https://en.wikipedia.org/wiki/JPEG_File_Interchange_Format#File_format_structure)
