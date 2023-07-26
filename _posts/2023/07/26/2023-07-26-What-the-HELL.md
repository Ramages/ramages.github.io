---
date: 2023-07-26 00:30:40 +02:00
categories: [SecurityValley, Coding]
tags: [Coding, CTF, SecurityValley]
---
This is a writeup of the coding challenge What the HELL from the [Security Valley](https://ctf.securityvalley.org) CTF

# Premise

All hell is breaking loose. Once again a frontend developer went completely nuts and left us with a JavaScrit project that nobody understands anymore...typical, these frontend developers...surely you can help us, right? Find the flag!

**Link:** [http://pwnme.org:8666](http://pwnme.org:8666/)

## Challenge code
### Website:
```html
<!DOCTYPE html>
<html>
    <head>
        <link rel="preconnect" href="https://fonts.googleapis.com">
        <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
        <link href="https://fonts.googleapis.com/css2?family=DotGothic16&display=swap" rel="stylesheet"> 
        <link rel="stylesheet" href="hell.css">
        <script src="hell.js"></script>
    </head>
    <body>
        <div class="content">
            <div>
                <h1>HELL'S PAGE</h1>
            </div>
            <div>
                <input type="text" id="hell-key" placeholder="Enter key to open the hell"/>
            </div>
            <div class="flag-content">
                <h1>Flag: <span id="flag-out"></span></h1>
            </div>
        </div>

    </body>
</html>
```
### hell.js:
```javascript
function changeHandler(e) {
    window.hell_key = e.target.value;
}


function main() {
    const cmd = [
        [
            "a",
            "696564797e2a6f662a372a6e65697f676f647e246d6f7e4f666f676f647e4873436e22286c666b6d27657f7e282331006f66246364646f785e6f727e2a372a6b31"
        ],
        [
            "",
            "7d63646e657d246c666b6d2a372a284770653b465e5f3c44633a704560537e4460653b465e4f3c44633a7247606538465e4f724560697e475e4b3c44633a704560697e475e5f3c44593a3f4560537e47706539465e4f3b45605f7e454e6538465e5f3c44633a7247606538465e4f734560537e475e5f3c44593a734560537e445e6539465e5b3c44733a72445e653b465e533c44633a3b4560697e475e5b3c44633a724560437e475e473c447d373728"
        ],
        [
            "",
            "2a2a2a2a2a2a2a2a666f7e2a686665682a372a646f7d2a4866656822516b7e656822284673326d6e52446650494862684d6e7c43493a6d434d44626863483f68395f6d69675c38505240705059487a6e4e32415338337f69395b6d53594b33434943255a703263496772666e49487c6e525b6d5a594b63436d7a6768394365684d5c3a434d616d5a594b7d4573487a434e7d6d53593f79505d3f646e4d6d3d434d61784173616d6f7d656d43494b6d684d5c3a4342586668524b6d5a594862466744655352404e683858665b525b656b59616d40634b7d6f4f504d49634b6d4349487950525b6d68494b334342586668524b6d40634b7d6f4e484d49634b6d4349487950525b6d6b494b334349623a505d3b7d434e3e21434e5b7a4349536d4742624d5860794149634b6d4349487a50634b65414d61784759616d5a5e3a6d53593f79505d3f646e4d6d7a4342794143494b6d43494b6d4349487c6e525b6d41703a6d68494b78436065634173486549634b6d43494833434d5c7969385f6d6f7d656d43494b6d43494b6d434d333b6e494b785a5948794349796345634378434d6d7843633a6349634b6d4349483349643a4128235726717e737a6f30282a6b7a7a6663696b7e636564257e6f727e287723002a2a2a2a002a2a2a2a2a2a2a2a696564797e2a6f666f676f647e2a372a6e65697f676f647e2469786f6b7e6f4f666f676f647e22286b282331002a2a2a2a2a2a2a2a6f666f676f647e24796f7e4b7e7e7863687f7e6f222862786f6c28262a7d63646e657d245f58462469786f6b7e6f4568606f697e5f58462268666568232331002a2a2a2a2a2a2a2a6f666f676f647e24796f7e4b7e7e7863687f7e6f22286e657d6466656b6e28262a28676f79796b6d6f556c78656755626f6666247e727e282331002a2a2a2a2a2a2a2a6f666f676f647e24797e73666f246e63797a666b732a372a286465646f2831002a2a2a2a2a2a2a2a6e65697f676f647e2468656e73246b7a7a6f646e496263666e226f666f676f647e2331002a2a2a2a2a2a2a2a6f666f676f647e246966636961222331002a2a2a2a2a2a2a2a6e65697f676f647e2468656e7324786f67657c6f496263666e226f666f676f647e2331"
        ]
    ]
    
    const l_cmd = [
    
    ]
    

    function preloader() {    
        for(let a = 0; a < cmd.length; a++) {

            let t = [];

            for(let i = 0; i < cmd[a][1].length; i += 2) {
                t.push(String.fromCharCode(parseInt(cmd[a][1].substr(i, 2), 16) ^ 0xA));
            }

            l_cmd.push(new Function(cmd[a][0],t.join("")))

        }
    }

    preloader();

    l_cmd[0]("closed...")
    l_cmd[1]()

    setInterval(() => {
        if (window.hell_key == "666") {
            l_cmd[0]("MESSAGE FROM HELL!")
            window.hell_key = "";
            l_cmd[2]()
        } else {
            l_cmd[0]("still closed...")
        }
    }, 1000);
}

document.addEventListener("DOMContentLoaded", () => {

    const input = document.getElementById("hell-key");
    input.addEventListener('input', changeHandler, false);

    main();
});
```

### message_from_hell.txt:
```javascript
// used algo -  can you reverse it?
const a = "???"
let out = ""
for(let i = 0; i < a.length; i++) {
    let temp = a.charCodeAt(i) & 0xFF
    let l = temp & 0x0F
    let h = (temp >> 4) & 0xFF;

    if ((i+1) == a.length) {
        out += l +":"+ h
    } else {
        out += l +":"+ h+"-"
    }
}
```
# Solution
At first in this challenge, we find a web page with a text input field. This doesn't give us too much information at first glance, so we move forwards by looking at the page source. An interesting line here is:
```html
<script src="hell.js"></script>
```
Which leads us to our next step. Here we find the section:
```javascript
l_cmd[0]("closed...")
l_cmd[1]()

setInterval(() => {
    if (window.hell_key == "666") {
        l_cmd[0]("MESSAGE FROM HELL!")
        window.hell_key = "";
        l_cmd[2]()
    } else {
        l_cmd[0]("still closed...")
    }
}, 1000);
```
As we can see, if we enter the `hell_key`, that being `666`, we get a textfile called `message_from_hell.txt`.
This file contains the algorithm used to encode messages. In it, we have a constant, `a` defined as "???", but we can use the value set to `a` from hell.js, that being: `"696564797e2a6f662a372a6e65697f676f647e246d6f7e4f666f676f647e4873436e22286c666b6d27657f7e282331006f66246364646f785e6f727e2a372a6b31"`

However, we also have the `preloader` function from hell.js. With the code:
```javascript
function preloader() {    
    for(let a = 0; a < cmd.length; a++) {

        let t = [];

        for(let i = 0; i < cmd[a][1].length; i += 2) {
            t.push(String.fromCharCode(parseInt(cmd[a][1].substr(i, 2), 16) ^ 0xA));
        }

        l_cmd.push(new Function(cmd[a][0],t.join("")))

    }
}
```
We can see that the string has been hex encoded and XORed with 0xA. More explicitly:
```
Hex encoding character into decimal format: parseInt(cmd[a][1].substr(i, 2), 16)
XORing with 0xA: parseInt(cmd[a][1].substr(i, 2), 16) ^ 0xA
Create a string from the decimal number: String.fromCharCode(parseInt(cmd[a][1].substr(i, 2), 16) ^ 0xA)
```
Therefore, if we first hex decode the `"696564797e2a6f662a372a6e65697f676f647e246d6f7e4f666f676f647e4873436e22286c666b6d27657f7e282331006f66246364646f785e6f727e2a372a6b31"` string, and then XOR it with 0xA, we get the following:
```javascript
const el = document.getElementById("flag-out");
el.innerText = a;
```
If we now do the same for the other two strings in the cmd array from hell.js, we get the following from the `7d63646e657d246c666b6d2a372a284770653b465e5f3c44633a704560537e4460653b465e4f3c44633a7247606538465e4f724560697e475e4b3c44633a704560697e475e5f3c44593a3f4560537e47706539465e4f3b45605f7e454e6538465e5f3c44633a7247606538465e4f734560537e475e5f3c44593a734560537e445e6539465e5b3c44733a72445e653b465e533c44633a3b4560697e475e5b3c44633a724560437e475e473c447d373728` string:
```javascript
window.flag = "Mzo1LTU6Ni0zOjYtNjo1LTE6Ni0xMjo2LTExOjctMTA6Ni0zOjctMTU6NS05OjYtMzo3LTE1OjUtODo2LTU6Ni0xMjo2LTEyOjYtMTU6NS0yOjYtNTo3LTQ6Ny0xNTo1LTY6Ni01OjctMTQ6Ni0xOjItMTM6Nw=="
```
Which as we can see is base64 encoded.

The third and final string in the cmd array is `2a2a2a2a2a2a2a2a666f7e2a686665682a372a646f7d2a4866656822516b7e656822284673326d6e52446650494862684d6e7c43493a6d434d44626863483f68395f6d69675c38505240705059487a6e4e32415338337f69395b6d53594b33434943255a703263496772666e49487c6e525b6d5a594b63436d7a6768394365684d5c3a434d616d5a594b7d4573487a434e7d6d53593f79505d3f646e4d6d3d434d61784173616d6f7d656d43494b6d684d5c3a4342586668524b6d5a594862466744655352404e683858665b525b656b59616d40634b7d6f4f504d49634b6d4349487950525b6d68494b334342586668524b6d40634b7d6f4e484d49634b6d4349487950525b6d6b494b334349623a505d3b7d434e3e21434e5b7a4349536d4742624d5860794149634b6d4349487a50634b65414d61784759616d5a5e3a6d53593f79505d3f646e4d6d7a4342794143494b6d43494b6d4349487c6e525b6d41703a6d68494b78436065634173486549634b6d43494833434d5c7969385f6d6f7d656d43494b6d43494b6d434d333b6e494b785a5948794349796345634378434d6d7843633a6349634b6d4349483349643a4128235726717e737a6f30282a6b7a7a6663696b7e636564257e6f727e287723002a2a2a2a002a2a2a2a2a2a2a2a696564797e2a6f666f676f647e2a372a6e65697f676f647e2469786f6b7e6f4f666f676f647e22286b282331002a2a2a2a2a2a2a2a6f666f676f647e24796f7e4b7e7e7863687f7e6f222862786f6c28262a7d63646e657d245f58462469786f6b7e6f4568606f697e5f58462268666568232331002a2a2a2a2a2a2a2a6f666f676f647e24796f7e4b7e7e7863687f7e6f22286e657d6466656b6e28262a28676f79796b6d6f556c78656755626f6666247e727e282331002a2a2a2a2a2a2a2a6f666f676f647e24797e73666f246e63797a666b732a372a286465646f2831002a2a2a2a2a2a2a2a6e65697f676f647e2468656e73246b7a7a6f646e496263666e226f666f676f647e2331002a2a2a2a2a2a2a2a6f666f676f647e246966636961222331002a2a2a2a2a2a2a2a6e65697f676f647e2468656e7324786f67657c6f496263666e226f666f676f647e2331`
```javascript
        let blob = new Blob([atob("Ly8gdXNlZCBhbGdvIC0gIGNhbiB5b3UgcmV2ZXJzZSBpdD8KY29uc3QgYSA9ICI/Pz8iCmxldCBvdXQgPSAiIgpmb3IobGV0IGkgPSAwOyBpIDwgYS5sZW5ndGg7IGkrKykgewogICAgbGV0IHRlbXAgPSBhLmNoYXJDb2RlQXQoaSkgJiAweEZGCiAgICBsZXQgbCA9IHRlbXAgJiAweDBGCiAgICBsZXQgaCA9ICh0ZW1wID4+IDQpICYgMHhGRjsKCiAgICBpZiAoKGkrMSkgPT0gYS5sZW5ndGgpIHsKICAgICAgICBvdXQgKz0gbCArIjoiKyBoCiAgICB9IGVsc2UgewogICAgICAgIG91dCArPSBsICsiOiIrIGgrIi0iCiAgICB9Cn0K")],{type:" application/text"})
    
        const element = document.createElement("a");
        element.setAttribute("href", window.URL.createObjectURL(blob));
        element.setAttribute("download", "message_from_hell.txt");
        element.style.display = "none";
        document.body.appendChild(element);
        element.click();
        document.body.removeChild(element);
```
Moving back to the 2nd string, the one encoded in base64, has the following content if we decode it:
`3:5-5:6-3:6-6:5-1:6-12:6-11:7-10:6-3:7-15:5-9:6-3:7-15:5-8:6-5:6-12:6-12:6-15:5-2:6-5:7-4:7-15:5-6:6-5:7-14:6-1:2-13:7`

This looks like the output of something run with the message_from_hell.txt encoding algorithm, so if we do what the comment said and reverse it, we get something like the following algorithm:
```javascript
const a = "3:5-5:6-3:6-6:5-1:6-12:6-11:7-10:6-3:7-15:5-9:6-3:7-15:5-8:6-5:6-12:6-12:6-15:5-2:6-5:7-4:7-15:5-6:6-5:7-14:6-1:2-13:7";
let out = "";
const pairs = a.split("-");
for (let i = 0; i < pairs.length; i++) {
  const pair = pairs[i].split(":");
  const l = parseInt(pair[0], 10);
  const h = parseInt(pair[1], 10);

  const temp = (h << 4) | l;
  out += String.fromCharCode(temp);
}
console.log(out);
```

Which prints out the flag
 `SecVal{[redacted]}`