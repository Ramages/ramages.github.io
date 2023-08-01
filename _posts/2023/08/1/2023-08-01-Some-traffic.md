---
date: 2023-07-31 17:58:22 +02:00
categories: [TFCCTF, Forensics]
tags: [Forensics, CTF, TFCCTF]
---
This is a writeup of the forensics challenge Some Traffic from the [TFC CTF](https://ctf.thefewchosen.com) 
#### Points: 50
# Premise
There might be a little **extra** piece of information here.


## Challenge files:

[sus.pcapng](https://drive.google.com/file/d/1YDtMUdKeNbv6QAIknzJdu9eMi6fvG3J7/view?usp=sharing)
(later: flag.png)

# Observations
Opening the file in wireshark, we can see a HTTP POST request with some png image data. 
We can use the filter `http && http.request.method == POST` to see this data more easily.
![filtered wshark](/assets/images/TFCCTF/sometraffic/filtered_wshark.png)

Viewing the full POST content, we can view the image data in ascii.

![image data](/assets/images/TFCCTF/sometraffic/ascii_png_package_data.png)
# Solution
We can change the data content to raw and search for the [PNG header](https://docs.fileformat.com/image/png/) (`89504e470d0a1a0a`).
![wshark raw img data](/assets/images/TFCCTF/sometraffic/raw_img_data.png)

We grab the data from the PNG header and continue all the way to the end of the data sent, and saving this to a file we'll name `flag.raw`.

We now need to convert the raw data to a png image, and we can do this with a simple python script:
```python
import sys

def file_extraction(filepath):
    file_data = open(filepath, "r").read()
    return file_data

def raw_to_png(filepath):
    hex_data = file_extraction(filepath)

    #Remove any trailing whitespace chars
    hex_data = hex_data.replace('\n', '').replace('\r', '')

    binary_data = bytes.fromhex(hex_data)

    with open("flag.png", "wb") as file:
        file.write(binary_data)
    
    
if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Error, usage is: raw_to_png.py file_with_raw_data")
    if len(sys.argv) == 2:
        raw_to_png(sys.argv[1])
```

Doing this creates a file named `flag.png`.

![flag_png](/assets/images/TFCCTF/sometraffic/flag.png)

We can continue investigating this flag png file.

If we analyzs the image with a tool like zsteg on the file, we get the following result:
![flag_png](/assets/images/TFCCTF/sometraffic/zsteg_flag.png)

Giving us our flag: `TFCCTF{H1dd3n_d4t4_1n_p1x3ls_i5n't_f4n_4nd_e4sy_to_f1nd!}`

# Tools and sources used:
 - [Wireshark](https://www.wireshark.org/)
 - [CyberChef](https://gchq.github.io/CyberChef)
 - [zsteg](https://github.com/zed-0xff/zsteg)
