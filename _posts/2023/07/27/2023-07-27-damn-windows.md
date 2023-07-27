---
date: 2023-07-27 22:48:54 +02:00
categories: [SecurityValley, Reversing]
tags: [Reversing, CTF, SecurityValley]
---
This is a writeup of the reversing challenge damn windows from the [Security Valley](https://ctf.securityvalley.org) CTF
#### Level: 3, Score: 40
# Premise
There is some exotic compiled windows executable.Can you deal with it?

**Link:** [https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/damn_windows](https://github.com/SecurityValley/PublicCTFChallenges/tree/master/reversing/damn_windows)
## Challenge files:
01.exe
# Observations
Simply running the program gives us the following output:
```
can you deal with a more complex language then C?
please enter the password
```
So we try the password `test`, which gives us the output:
```
test
2023/07/27 21:53:26 wrong password kid
```
So no luck.

Fiddling around with the input doesn't yield any results, so now another method is required.

# Solution
If we investigate the file in the tool [Exeinfo PE](https://github.com/ExeinfoASL/ASL), it tells us that the program is written in [GOlang](https://go.dev/). Moving forwards, we open the program in [ghidra](https://github.com/NationalSecurityAgency/ghidra), run its analyzer and investigate further. 
At a glance, we find ourselves having a hard time getting further, but if we run the [GO function renamer script for ghidra](https://github.com/getCUJO/ThreatIntel/blob/master/Scripts/Ghidra/go_func.py) written by [Dorka Patolay](https://cujo.com/reverse-engineering-go-binaries-with-ghidra/), we're able to investigate more easily.

If we look at the main.main function, we find the following content:
```go
void main.main(void)

{
  undefined8 extraout_RAX;
  undefined8 extraout_RAX_00;
  undefined8 uVar1;
  int extraout_RCX;
  int extraout_RCX_00;
  undefined8 extraout_RBX;
  undefined8 extraout_RBX_00;
  undefined8 uVar2;
  undefined8 extraout_RDI;
  undefined auVar3 [16];
  undefined auVar4 [16];
  undefined8 local_118;
  int local_110;
  undefined8 local_108;
  undefined8 local_100;
  undefined local_f8 [16];
  undefined local_e8 [16];
  undefined local_d8 [16];
  undefined local_c8 [16];
  undefined local_b8 [16];
  undefined8 local_a8;
  runtime.itab *local_a0;
  undefined8 local_98;
  undefined8 local_70;
  undefined8 local_68;
  undefined local_60 [88];
  
  auVar3 = (undefined  [16])0x0;
  while (local_c8 + 8 <= CURRENT_G.stackguard0) {
    runtime.morestack_noctxt();
  }
  local_d8._8_8_ = &PTR_DAT_004d37f8;
  local_d8._0_8_ = &string___runtime._type;
  fmt.Fprintln(&go.itab.*os.File,io.Writer,os.Stdout,local_d8,1,1);
  local_e8._8_8_ = &PTR_DAT_004d3808;
  local_e8._0_8_ = &string___runtime._type;
  fmt.Fprintln(&go.itab.*os.File,io.Writer,os.Stdout,local_e8,1,1);
  local_100 = os.Stdin;
  local_60._0_16_ = auVar3;
  FUN_0045d0d0();
  runtime.makeslice(&uint8___runtime._type,0x1000,0x1000);
  local_b8 = auVar3;
  local_b8._0_8_ = FUN_0045d0d0();
  local_b8._8_8_ = 0x1000;
  local_a8 = 0x1000;
  local_a0 = &go.itab.*os.File,io.Reader;
  local_98 = local_100;
  local_70 = 0xffffffffffffffff;
  local_68 = 0xffffffffffffffff;
  local_60._0_8_ = local_b8._0_8_;
  FUN_0045d43a();
  bufio.(*Reader).ReadString(local_60,10);
  local_110 = extraout_RCX;
  auVar4 = strings.TrimRight(extraout_RAX,extraout_RBX,&DAT_004afbcc,2);
  if (local_110 == 0) {
    main.(*PasswordService).Validate(&local_118,auVar4._0_8_,auVar4._8_8_);
    uVar1 = extraout_RAX_00;
    uVar2 = extraout_RBX_00;
    if (extraout_RCX_00 != 0) {
      local_118 = extraout_RBX_00;
      local_108 = extraout_RAX_00;
      (**(code **)(extraout_RCX_00 + 0x18))(extraout_RDI);
      local_c8 = auVar3;
      local_c8._8_8_ = runtime.convTstring();
      local_c8._0_8_ = &string___runtime._type;
      log.Fatal(local_c8,1,1);
      uVar1 = local_108;
      uVar2 = local_118;
    }
    local_f8 = auVar3;
    local_f8._8_8_ = runtime.convTstring(uVar1,uVar2);
    local_f8._0_8_ = &string___runtime._type;
    fmt.Fprintln(&go.itab.*os.File,io.Writer,os.Stdout,local_f8,1,1);
  }
  bufio.(*Reader).ReadString(local_60,10);
  return;
}
```
We find the string:
```go
main.(*PasswordService).Validate(&local_118,auVar4._0_8_,auVar4._8_8_);
```
Which we investigate further. Going to this function, we find the following content:
```go
undefined  [32] main.(*PasswordService).Validate(undefined8 param_1,undefined8 param_2,int param_3)

{
  char cVar1;
  int iVar2;
  undefined8 *puVar3;
  undefined auVar4 [32];
  undefined auVar5 [16];
  undefined8 param_11;
  undefined local_48 [16];
  undefined local_38 [16];
  undefined local_28 [16];
  undefined local_18 [16];
  
  while (param_11 = param_2, local_18 + 8 <= CURRENT_G.stackguard0) {
    runtime.morestack_noctxt();
    param_2 = param_11;
  }
  local_48._8_8_ = 6;
  local_48._0_8_ = &DAT_004b00a8;
  local_38._8_8_ = 1;
  local_38._0_8_ = &DAT_004afb6e;
  local_28._8_8_ = 0xc;
  local_28._0_8_ = &DAT_004b1275;
  local_18._8_8_ = 1;
  local_18._0_8_ = &DAT_004afb6f;
  auVar5 = strings.Join(local_48,4,4,0,0);
  if ((auVar5._8_8_ == param_3) && (cVar1 = runtime.memequal(param_2,auVar5._0_8_), cVar1 != '\0'))
  {
    iVar2 = 0;
  }
  else {
    iVar2 = runtime.cmpstring(param_2,param_3,auVar5._0_8_,auVar5._8_8_);
    if (iVar2 < 0) {
      iVar2 = -1;
    }
    else {
      iVar2 = 1;
    }
  }
  if (iVar2 != 0) {
    puVar3 = (undefined8 *)runtime.newobject(&errors.errorString___runtime.structtype);
    puVar3[1] = 0x12;
    *puVar3 = &DAT_004b26d7;
    auVar4._0_16_ = ZEXT816(0);
    auVar4._16_8_ = &go.itab.*errors.errorString,error;
    auVar4._24_8_ = puVar3;
    return auVar4;
  }
  return ZEXT1632(CONCAT88(8,&DAT_004b04ef));
}
```
The 8 sections named `local_18`, `local_28`, `local_38` and `local_48` with a trailing 0 or 8 point to specific data sections, we can follow the top one of these, that being `local_48` which points to `DAT_004b00a8`. 
Here we find the following content:
```
DAT_004b00a8                    XREF[2]:     main.(*PasswordService).Validate
main.(*PasswordService).Validate
        004b00a8 53              ??         53h    S
        004b00a9 65              ??         65h    e
        004b00aa 63              ??         63h    c
        004b00ab 56              ??         56h    V
        004b00ac 61              ??         61h    a
        004b00ad 6c              ??         6Ch    l
```
Which contains the first part required for our flag. If we keep looking at these reference points, we additionally find the left curly brace for our flag form the `local_38._0_8_` variable
```
DAT_004afb6e                                    XREF[3]:     
runtime.printArgs:00452cc5(*),
main.(*PasswordService).Validate
        004afb6e 7b              ??         7Bh    {

```
and the right curly brace for our flag from the `local_18._0_8_` variable
```
DAT_004afb6f                                    XREF[3]:
runtime.printArgs:00452d99(*), 
main.(*PasswordService).Validate
        004afb6f 7d              ??         7Dh    }

```
Now the string stored in the `local_28._0_8_` variable contain our flag, so I will refrain from showing its content here.

However, if the flag is entered into the 01.exe file, the full conversation looks like this:
```
can you deal with a more complex language then C?
please enter the password
SecVal{[REDACTED]}
correct!


```
# Tools used:
 - cmd.exe
 - [Exeinfo PE](https://github.com/ExeinfoASL/ASL)
 - [ghidra](https://github.com/NationalSecurityAgency/ghidra)
 - [GO function renamer script for ghidra](https://github.com/getCUJO/ThreatIntel/blob/master/Scripts/Ghidra/go_func.py)