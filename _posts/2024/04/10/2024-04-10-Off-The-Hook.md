---
date: 2024-04-10 21:21:37 +02:00
categories: [swampCTF, Rev]
tags: [Rev, CTF, swampCTF]
---
This is a writeup of the Pwn challenge Off The Hook by [swampCTF](https://swampctf.com/) 
#### Points: 300
# Premise
We did some dabbling with apk's, have fun!

## Challenge files:
[swamp.apk](https://ctf.swampctf.com/files/5b8baa194a7daca62f1b3e461904d497/swamp.apk)

# Observations
We get a single apk file. Luckily, [apks](https://docs.fileformat.com/compression/apk/) are in a way like ZIP folders, so we can unzip the file, and extract the following files:
![apk contents](/assets/images/swampCTF/hook/content.png)

Of interest to us here, is the ```classes.dex``` file, which when opened in [jadx](https://github.com/skylot/jadx) allows us to view the following:
![main function](/assets/images/swampCTF/hook/jadxmain.png)

As we can see, the flag is obfuscated, and becomes deobfuscated when the line:
```java
return  "swampCTF{"  +  new  String(GingerBread.gumdrop(Base64.decode("otP+wTo2MYdv8+Uv5Gyoqg==",  0),  bytes),  StandardCharsets.UTF_8)  +  "}";
```
is run.
However, as we can see in the line, we need to pass it through the class ```GingerBread```s ```gumdrop``` function, which we can see a snippet of here:
![gumdrop function](/assets/images/swampCTF/hook/gumdrop.png)

# Solution
Now that we have the source code that generates the flag, we can get to work. To get the flag back, we're going to need a way to run some java code, which can either be done in your IDE of choice, or in this case, with a site like [jdoodle](https://www.jdoodle.com/online-java-compiler-ide/).

Copying over the ```MainActivity``` and ```GingerBread``` classes, we need to slightly tweak them before we can run the program.

In ```GingerBread```, the value ```UByte.MAX_VALUE``` is changed ```255```.

In ```MainActivity``` we only copy over the contents of the ```public String getFlag()``` function.

We then remove the line ```verify("Onions");```

We also remove ```StandardCharsets.UTF_8``` from the code entierly, and change the last line, which initially is a return value, to a print statement. Furthermore, we import the Base64 library and change the line ```Base64.decode("otP+wTo2MYdv8+Uv5Gyoqg==",  0)``` to just be a string.

After all of this, we now have the following main function:
```java
import java.util.Base64;
public class Main {
    public static void main(String args[]) {
      byte[] bytes = String.format("%1$-32s", "MUFFIN_MAN").getBytes();
      byte[] flagBytes = Base64.getDecoder().decode("otP+wTo2MYdv8+Uv5Gyoqg==");
      
      GingerBread.S = GingerBread.generateSubkeys(bytes);
      System.out.println("swampCTF{" + new String(GingerBread.gumdrop(flagBytes, bytes)) + "}");
    }
}
```

Running this, together with the tweaked ```GingerBread``` class gives us the following:

![gumdrop function](/assets/images/swampCTF/hook/win.png)

So we now have our flag:
> swampCTF{TH@TLL_D0_D0NK!}
{: .prompt-tip }
# Tools and sources used:
- [jadx](https://github.com/skylot/jadx)
- [jdoodle](https://www.jdoodle.com/online-java-compiler-ide/)
