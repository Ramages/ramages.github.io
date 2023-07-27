---
date: 2023-07-27 20:45:01 +02:00
categories: [SecurityValley, Reversing]
tags: [Reversing, CTF, SecurityValley]
---
This is a writeup of the reversing challenge Forgotten lisence key from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 3, Score: 30
# Premise
Unfortunately, I forgot my license key. Fortunately, the program is somewhat chatty. Maybe together we can manage to recover the license key. Can you help me?

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/forgotten_license_key](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/forgotten_license_key)
## Challenge files:
license
license.exe
# Observations
Simply running the program gives us the following output:
```
license: error: required flag --key not provided, try --help
```
So we add the --help flag and receive the following output:
```
usage: license --key=KEY [<flags>]

Flags:
  --help     Show context-sensitive help (also try --help-long and --help-man).
  --key=KEY  License key
```
As we can see, we need to supply a key. We can try running something like `./lisence --key=test`, which gives us the following output:
```
wrong length
```
After some trial and error, we end up finding the correct length of the key, that being 9.
However, when running `./lisence --key=aaaaaaaaa` we get the output:
```
expect "-" at position 4
```
So we update our key and try again by running `./lisence --key=aaaa-aaaa`, which gives us the output:
```
expect "7" at position 0
```
We update our key once again and try running `./lisence --key=7aaa-aaaa`, which gives us the output:
```
expect "D" at position 6
```
We keep trying and run `./lisence --key=7aaa-aDaa`, which gives us the output:
```
checksum for first license key block failed
```
Here we've reached the limit of what the program is willing to give us, so now another method is required.

# Solution
If we investigate the file in the tool [Exeinfo PE](https://github.com/ExeinfoASL/ASL), it tells us that the program is written in [GOlang](https://go.dev/). Moving forwards, we open the program in [ghidra](https://github.com/NationalSecurityAgency/ghidra), run its analyzer and investigate further. 
At a glance, we find ourselves having a hard time getting further, but if we run the [GO function renamer script for ghidra](https://github.com/getCUJO/ThreatIntel/blob/master/Scripts/Ghidra/go_func.py) written by [Dorka Patolay](https://cujo.com/reverse-engineering-go-binaries-with-ghidra/), we're able to investigate more easily.
If we look at the main.main function, we find the following content:
```go
void main.main(void)
{
  undefined8 uVar1;
  undefined auVar2 [16];
  undefined local_38 [16];
  undefined local_28 [16];
  undefined local_18 [16];
  
  local_38 = (undefined  [16])0x0;
  while (&stack0x00000000 <= CURRENT_G.stackguard0) {
    runtime.morestack_noctxt();
  }
  gopkg.in/alecthomas/kingpin%2ev2.Parse();
  uVar1 = *DAT_006f0e30;
  if (DAT_006f0e30[1] == 9) {
    auVar2 = main.validateLicenseFormat();
    if (auVar2._0_8_ != 0) {
      (**(code **)(auVar2._0_8_ + 0x18))(auVar2._8_8_);
      local_28 = local_38;
      local_28._8_8_ = runtime.convTstring();
      local_28._0_8_ = &string___runtime._type;
      fmt.Fprintln(&io.Writer___runtime.itab,DAT_006f0e78,local_28,1,1);
      return;
    }
    auVar2 = main.loadStaticLicense(uVar1,9);
    runtime.slicebytetostring(0,auVar2._0_8_,auVar2._8_8_);
    local_38._8_8_ = runtime.convTstring();
    local_38._0_8_ = &string___runtime._type;
    fmt.Fprintln(&io.Writer___runtime.itab,DAT_006f0e78,local_38,1,1);
    return;
  }
  local_18._8_8_ = &PTR_DAT_005e5d18;
  local_18._0_8_ = &string___runtime._type;
  fmt.Fprintln(&io.Writer___runtime.itab,DAT_006f0e78,local_18,1,1);
  return;
}
```
We can see a call being made to a function named `main.validateLisenceFormat` which sounds promising, so we move into it to investigate further. 
Navigating to the `main.validateLisenceFormat` function, we find the following contents:
```go
undefined  [16] main.validateLicenseFormat(char *param_1,uint param_2)

{
  undefined8 *puVar1;
  undefined auVar2 [16];
  undefined auVar3 [16];
  undefined auVar4 [16];
  undefined auVar5 [16];
  undefined auVar6 [16];
  char *param_10;
  
  param_10 = param_1;
  while (&stack0x00000000 <= CURRENT_G.stackguard0) {
    runtime.morestack_noctxt();
  }
  if (param_2 < 5) {
                    /* WARNING: Subroutine does not return */
    runtime.panicIndex(4,param_2,param_2);
  }
  if (param_10[4] != '-') {
    puVar1 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
    puVar1[1] = 0x18;
    *puVar1 = &DAT_005ac18a;
    auVar6._8_8_ = puVar1;
    auVar6._0_8_ = &error___runtime.itab;
    return auVar6;
  }
  if (*param_10 != '7') {
    puVar1 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
    puVar1[1] = 0x18;
    *puVar1 = &DAT_005ac1a2;
    auVar5._8_8_ = puVar1;
    auVar5._0_8_ = &error___runtime.itab;
    return auVar5;
  }
  if (param_2 < 7) {
                    /* WARNING: Subroutine does not return */
    runtime.panicIndex(6,param_2,param_2);
  }
  if (param_10[6] != 'D') {
    puVar1 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
    puVar1[1] = 0x18;
    *puVar1 = &DAT_005ac1ba;
    auVar4._8_8_ = puVar1;
    auVar4._0_8_ = &error___runtime.itab;
    return auVar4;
  }
  if ((dword)((byte)param_10[1] + 0x37 + (dword)(byte)param_10[2] + (dword)(byte)param_10[3]) !=
      0x102) {
    puVar1 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
    puVar1[1] = 0x2b;
    *puVar1 = &DAT_005b14fc;
    auVar3._8_8_ = puVar1;
    auVar3._0_8_ = &error___runtime.itab;
    return auVar3;
  }
  if (7 < param_2) {
    if (param_2 < 9) {
                    /* WARNING: Subroutine does not return */
      runtime.panicIndex(8,param_2,param_2);
    }
    if ((dword)((dword)(byte)param_10[8] + (byte)param_10[5] + 0x44 + (dword)(byte)param_10[7]) !=
        0x11a) {
      puVar1 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
      puVar1[1] = 0x2c;
      *puVar1 = &DAT_005b195c;
      auVar2._8_8_ = puVar1;
      auVar2._0_8_ = &error___runtime.itab;
      return auVar2;
    }
    return ZEXT816(0);
  }
                    /* WARNING: Subroutine does not return */
  runtime.panicIndex(7,param_2,param_2);
}
```
In the early if statements, we find what looks to be the checks the program performs on the license key. 
These checks are done towards the variable `param_10`, so we rename it to `license_key_arr` to make investigations a little easier.

The lines checking if all variables in the key equate to `0x102` and `0x11a`, we convert these to decimal format, and change the variables `0x37` and `0x44` to char format, giving us `7` and `D` respectively.

The result of these changes give us:
`if ((dword)((byte)license_key_arr[1] + '7' + (dword)(byte)license_key_arr[2] + (dword)(byte)license_key_arr[3]) != 258)`
and
`if ((dword)((dword)(byte)license_key_arr[8] + (byte)license_key_arr[5] + 'D' + (dword)(byte)license_key_arr[7]) != 282)`

As we can see, we need to get the decimal values of the first key block to equate to 258, and 282 for the second key block.

We can write a bruteforcing script in python to help us with this. 
By taking the possible combinations of characters that, together with `7`, make `258`, and likewise for the second block that, together with `D` make `282`, we can attempt to find the key.

Furthermore, we can look for the string `SecVal` to avoid "bad flags".

In practice, this could look something along the lines of the following:
```python
import os
import string

the_key = "7aaa-aDaa"
output = "checksum for first license key block failed"
alphabet = list(string.ascii_letters)
numbers = ['0','1','2','3','4','5','6','7','8','9']
all_chars = alphabet + numbers

block_1_1, block_1_2, block_1_3 = '', '', ''

block_2_1, block_2_2, block_2_3 = '', '', ''

possible_combos = []
possible_combos_2 = []

for i in range(len(all_chars)):
  for j in range(len(all_chars)):
    for k in range(len(all_chars)):
      block_1_1 = all_chars[i]
      block_1_2 = all_chars[j]
      block_1_3 = all_chars[k]
      if(ord(block_1_1) + ord(block_1_2) + ord(block_1_3) == 203):
        possible_combos.append(block_1_1+block_1_2+block_1_3)

for i in range(len(all_chars)):
  for j in range(len(all_chars)):
    for k in range(len(all_chars)):
      block_2_1 = all_chars[i]
      block_2_2 = all_chars[j]
      block_2_3 = all_chars[k]
      if(ord(block_2_1) + ord(block_2_2) + ord(block_2_3) == 214):
        possible_combos_2.append(block_2_1+block_2_2+block_2_3)

for i in range(len(possible_combos)):
  for j in range(len(possible_combos_2)):
    the_key = "7"
    the_key = the_key + possible_combos[i]
    the_key = the_key + "-"
    the_key = the_key + possible_combos_2[j][0]
    the_key = the_key + "D"
    the_key = the_key + possible_combos_2[j][1] + possible_combos_2[j][2]
    output = os.popen(f"./license --key={the_key}").read()
    if("SecVal" in output):
      print(output)
      print("Key is: " + the_key)
      break
```
The output if this script is:
SecVal{[REDACTED]}

Key is: 7xxx-xDxx

Which gives us the key and the flag (which I've redacted from here)

# Tools used:
 - a bash shell
 - python3
 - [Exeinfo PE](https://github.com/ExeinfoASL/ASL)
 - [ghidra](https://github.com/NationalSecurityAgency/ghidra)
 - [GO function renamer script for ghidra](https://github.com/getCUJO/ThreatIntel/blob/master/Scripts/Ghidra/go_func.py)