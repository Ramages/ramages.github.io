---
date: 2024-04-10 20:41:29 +02:00
categories: [swampCTF, Pwn]
tags: [Pwn, CTF, swampCTF]
---

This is a writeup of the Pwn challenge Beginner Pwn 1 by [swampCTF](https://swampctf.com/) 
#### Points: 25
# Premise
Pwn can be a pretty intimidating catagory to get started in. So we made a few chals to help new comers get their feet wet!

`nc chals.swampctf.com 61230`
## Challenge files:

[main.c](https://ctf.swampctf.com/files/343a067c0daf2155dd6838d4d8eb350e/main.c)

[system_terminal](https://ctf.swampctf.com/files/9ee973435652979c8ffe33869f53b406/system_terminal)

# Observations
From the challenge files, we have the source code of the program, the program itself, and a netcat address pointing to where the live challenge is being run from.

main.c has the following contents:
```c
#include  <stdio.h>
#include  <stdlib.h>

void  print_flag();
void  print_ufsit_info();
void  print_pwn_info();

int  main() {
	int  user_option;
	int  is_admin  =  0;
	char  username[15];

	setbuf(stdin, NULL);
	setbuf(stdout, NULL);

	printf("Please enter your username to login.\n"
			"Username:");
	// I saw something online about this being vulnerable???
	// The blog I read said something about how a buffer overflow could corrupt other variables?!?
	// Eh whatever, It's probably safe to use here.
	gets(username);

	printf("Welcome %s!\n\n", username);

	if(is_admin  ==  0){

		printf("User %s is not a system admin!\n\n", username);

	} else {

		printf("User %s is a system admin!\n\n", username);

	}

	printf("In order to run a command, type the number and hit enter!\n");

	// This loop repeats forever
	// I hope the user never want's to log out
	while(1){
		printf("What command would you like to run?\n"
				"1 - Print Information About UFSIT\n"
				"2 - Print Information About Binary Exploitation (PWN)\n"
				"3 - Print The Flag\n"
				"4 - Exit The Program\n"
				">");
	
	scanf("%d", &user_option);

	if(user_option  ==  1){
		print_ufsit_info();
	}

	if(user_option  ==  2){
		print_pwn_info();
	}

	if(user_option  ==  3){
		// Check to see if the user is not an admin.
		if(is_admin  ==  0){
				printf("Sorry! You are not an admin!\n");
			} else{
				print_flag();
			}
		}
	if(user_option  ==  4){
		printf("Goodbye!\n");
		exit(0);
	}
	printf("\n");
	}
}

  

void  print_flag(){
	FILE*  ptr;
	char  str[50];
	ptr  =  fopen("flag.txt", "r");

	if (NULL  ==  ptr) {
		printf("file can't be opened, please let SwampCTF admins know if you see this!\n");
		exit(1);
	}
	printf("Here is your flag!\n");
	while (fgets(str, 50, ptr) !=  NULL) {
		printf("%s\n", str);
	}

	fclose(ptr);
	return;
}

  

void  print_ufsit_info(){
	printf("UFSIT is UFs cybersecurity and hacking club!\n\n"
		"Discord: https://discord.gg/7HFp3fVWJh\n"
		"Instagram: https://www.instagram.com/uf.sit/\n"
		"Website: https://www.ufsit.club/\n"
		"\n"
		"UFSIT is the beginner friendly cybersecurity club at the University of Florida. Our goal is to help get "
		"student interested in the field!"
		"\n");
	return;
}

  

void  print_pwn_info(){
		printf("Binary exploitation is the art of subverting the expectations of the original 			programmer. By providing "
		"a program with input that it doesn't know how to handle you can bend it until it breaks. "
		"This could be in the form of exploiting a logic bug or spotting something that the original programmer missed. "
		"When people think of hacking they think of binary exploitation. Now this may sound intimidating, but sometimes"
		" it's just a simple as overflowing a buffer."
		"\n\n"
		"Hacking isn't a toolset, it's a mindset.\n");
	return;
}
```
# Solution
Looking at the code, both from investigation and inference, we can gather that the ```gets(username)``` section is [vulnerable](https://faq.cprogramming.com/cgi-bin/smartfaq.cgi?answer=1049157810&id=1043284351). We also see that the ```char username[15]``` variable takes at most 15 characters.

Attempting to input a username longer than 15 characters gives us the following output: 
![Connecting to the challenge and overloading the buffer](/assets/images/swampCTF/pwn/win.png)

Which gives us the flag:
> swampCTF{y0u_@r3_a_h@ck3r}
{: .prompt-tip }

The reason this happens is because, if we look at the source code, we can see the variable declaration at the beginning being the following:
```c
int  user_option;
int  is_admin  =  0;
char  username[15];
```
As the contents of username exceed the memory space for that array, (in this case by holding more chars than intended), it "bleeds" into the ```is_admin = 0``` variable, setting it to a value > 0, which in turn, as the previous image showed, allows us to masquerade as an admin, and allows us to run the 3rd option from the challenge, spitting out the flag. A more thorough explanation of buffer overflows can be found [here](https://www.coengoedegebure.com/buffer-overflow-attacks-explained/)

# Tools and sources used: