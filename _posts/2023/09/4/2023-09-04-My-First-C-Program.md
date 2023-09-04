---
date: 2023-09-04 18:55:37 +02:00
categories: [DUCTF, misc]
tags: [misc, CTF, DUCTF]
---
This is a writeup of the misc challenge my first C program from the [DUCTF](https://play.duc.tf/) CTF
#### Level: easy, Score: 100
# Premise
I decided to finally sit down and learn C, and I don't know what all the fuss is about this language it writes like a _**dream**_!

Here is my first challenge in C! Its really easy after you install the C installer installer, after that you just run it and you're free to fly away with the flag like a _**berd**_!
## Challenge files:
my_first_c_program.c
# Observations
When opening the file, we're greeted with the following code:
```C
===== thunk.c ======

ncti thonk(a, b) => {
   const var brain_cell_one = "Th"!
   const const const const bc_two = "k"!
   const const var rett = "${brain_cell_one}}{$a}{b}!}!"!!
   const var ret = "${brain_cell_one}${a}${b}${bc_two}"!
   return ret!!!
   return rett!
}

export thonk to "flag.c"

===== flag.c =====
import thonk!

   union print_flag(end, middle, secondmiddle, start, realstart) => {
print("The flag is:")!
print("DUCTF{${start}_${realstart}_${end}_${secondmiddle}_1s_${middle}_C}")!!!
   }

ction get_a_char() => {
   const var dank_char = 'a'!
   if (;(7 ==== undefined)) {
      dank_char = 'I'!!
   }
   when (dank_char == 22) {
      print("Important 3 indentation check AI")!
      dank_char = 'z'!
   }
   if ( dank_char == 'j' ) {
      dank_char = 'c'!!
   }
   if ( 1.0 ==== 1.0) {
      dank_char = 'A'!!
   }

   return previous dank_char!
}

   func guesstimeate() => {
print('Thunking')!
print("life times ain't got nothign on rust!")!
print("The future: ${name}!")
const const name<-1> = "Pix"!
const const letter = 'n'
letter = 'p'
const var guess = "${previous letter}T"!
guess = "T${letter}${guess}"!
return previous guess!
   }

   fn get_undefined() => {
return (3 / 0)!       // Gives us get_undefined var!
   }

   fnc looper() => {
const var timeout: Int9 = 15!

print("Wait.. where are the loops?..")!

return timeout!
   }

function set_signal_fr(figsig) => {
   const var [[[getSig, setSig], setSig], setSig] = use(figsig)!
}

   fun math() => {
print("MatH!")
return 10 % 5
   }

   functi main() => {
print("Runnign my first C program!")!

print("It was so hard to install.. getting the C installer installer...
print( "cool that i don't have to finish prints if I get lazy!")!
set_signal_fr(0)!
const var end = "th${looper()}"!
print("C has cool timeout vars!")
const const name<5> = "'Program has started!'"!!!!!!
print(name)!

const const const vars = ["R34L", "T35T", "Fl4g", "variBl3"]

print("I always hated the number 8..")!
delete 8!!!!!!!!
const var heck_eight = get_a_char()!

const const thank = thonk(1, 'n')!

print("Everyone complains about null... sooo..")!
delete null!
print("Billion dollar mistake gone!")!

const const var ntino = "D${math()}${guesstimeate()}"

print("I should loop until flag print still")!
const const timer = looper()
// Now to print the flag for the CTF!!
print_flag(thank, vars[-1], end, heck_eight, ntino)

print("Done! Now for a beer")!
return 0!!!!!!!!!!!!!!!!!!!!
   }

===== test.c ======

uion main() => {
   print("Testing C here!")!!
   print("TODO: Actually write test harness...")!
}
```

As we can see, the code is not really C, but pseudo like enough to where we are able to guesstimate and deduct what it does.

# Solution

The line
```
// Now to print the flag for the CTF!!
print_flag(thank, vars[-1], end, heck_eight, ntino)
```
Looks to be the function that actually prints the flag, so we need to find the value for each of the variables.
Taking them in order, our first step is to find the value of the `thank` variable. 
The line:  `const const thank = thonk(1, 'n')!`  is where it is assigned. 
If we then go to the function, we see the following content:
```
ncti thonk(a, b) => {
   const var brain_cell_one = "Th"!
   const const const const bc_two = "k"!
   const const var rett = "${brain_cell_one}}{$a}{b}!}!"!!
   const var ret = "${brain_cell_one}${a}${b}${bc_two}"!
   return ret!!!
   return rett!
}
```

Looking at the ret variable (which is the variable that would be returned in this case), we can insert the values supplied by `thank` and within the function to get the string: `Th1nk`.
We know have our first variable for the flag.

Next, we need to find `vars[-1] `. 
vars is declared as: `const const const vars = ["R34L", "T35T", "Fl4g", "variBl3"]`.
If we go by python-like logic (which I tried at first), we find that vars would be `variBl3`, but to make a guessy story short, its `R34L`.

The next variable we need to find is `end`.
This is given by `const var end = "th${looper()}"!`. To get the full value of the variable, we need to find what `looper()` gives us. We find that `looper()` is given by:
```
   fnc looper() => {
const var timeout: Int9 = 15!

print("Wait.. where are the loops?..")!

return timeout!
   }
```
Which returns `15`, making `end`s value: `th15`.

The next variable is: `heck_eight`. Its value is given by: `const var heck_eight = get_a_char()!`, meaning that we need to find the value given by `get_a_char()`. `get_a_char()` is given by: 
```
ction get_a_char() => {
   const var dank_char = 'a'!
   if (;(7 ==== undefined)) {
      dank_char = 'I'!!
   }
   when (dank_char == 22) {
      print("Important 3 indentation check AI")!
      dank_char = 'z'!
   }
   if ( dank_char == 'j' ) {
      dank_char = 'c'!!
   }
   if ( 1.0 ==== 1.0) {
      dank_char = 'A'!!
   }

   return previous dank_char!
}
```
We can see that `dank_char` is first assigned to `'a'`, then `I`, since it wouldn't be changed by the `dank_char == 22` or `dank_char == 'c'`, and lastly to `'A'`. Since the function returns the previous char, it would return `'I'`, making the value of `dank_char` and by extension `heck_eight`: `'I'`.

This leaves us with our last variable needed to puzzle the flag together: `ntino`, which is given by:
`const const var ntino = "D${math()}${guesstimeate()}"`
so here, we need to find the values of `math()` and `guesstimeate()`. `math()` is given by: 
```
   fun math() => {
print("MatH!")
return 10 % 5
   }
```
Which would return 0 (since 10 modulo 5 is 0), making the value of `math()` `0`.
`guesstimeate()` is given by 
```
   func guesstimeate() => {
print('Thunking')!
print("life times ain't got nothign on rust!")!
print("The future: ${name}!")
const const name<-1> = "Pix"!
const const letter = 'n'
letter = 'p'
const var guess = "${previous letter}T"!
guess = "T${letter}${guess}"!
return previous guess!
   }
```
this function returns the variable `guess`, which as we can see in the first return statement consists of the previous value of the `letter` variable, which in this case would be `n`, and the letter `T`, making the value of `guess`: `nT`. Adding all of this together, gives us the value of `ntino`, which is `D0nT`.


We now have all variables needed to put our flag together with the function:
`print_flag(thank, vars[-1], end, heck_eight, ntino)`'
Remember that:
 * `thank = Th1nk`
 * `vars[-1] = R34L`
 * `end = th15`
 * `heck_eight = I`
 * `ntino = D0nT`
 
 If we now look at the `print_flag()` function, we can see that it is:
```
    union print_flag(end, middle, secondmiddle, start, realstart) => {
print("The flag is:")!
print("DUCTF{${start}_${realstart}_${end}_${secondmiddle}_1s_${middle}_C}")!!!
   }
 ```
 
 Which allows us to simply put the right variables in the right location
```
    union print_flag(thank, vars[-1], end, heck_eight, ntino) => {
print("The flag is:")!
print("DUCTF{${I}_${D0nT}_${Th1nk}_${th15}_1s_${R34L}_C}")!!!
   }
 ```
  
Giving us our flag
> DUCTF{I_D0nT_Th1nk_th15_1s_R34L_C}
{: .prompt-info }

# Tools used:
 - Any text editor (optional)
