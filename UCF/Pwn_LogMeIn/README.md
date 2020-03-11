# Log Me In!
```diff
- Still in progress
```

**Category:** Pwn

**Points:** 15

**Description:**

Author: kcolley

nc ctf.hackucf.org 7006

> **Files:** logmein, libpwnableharness64.so

## Write-up
When running the program, you are asked for a username and password:
```
> nc ctf.hackucf.org 7006
user = 0x1947010
Enter your username:
fred
Enter password for user: fred
fred
Invalid login information.
```

We clearly need to find out how to manipulate the input to successfully login.

I moved to using the local copy. First I tried entering nothing at the prompts:
```
> ./logmein
user = 0x87e010
Enter your username:

Enter password for user: 

Invalid login information.
```

I opened the local copy up in IDA Pro. There is a `handle_connection()` function, so that's probably the place to concentrate effort:

![handle_connection() function](handle_connection1.png)

> Key: The main function getting the input is in blue, the bit we need to get into is in green and the invalid state is in red (the dark red line is just a breakpoint).

We can see near the boundary between the blue and green is the test that decides which reply we get:
```assembly
mov    eax, [rax]
test   eax, eax          ; Tests (ANDs) the contents of eax (the input)
jz     short loc_400AB5  ; This jumps to the 'invalid' code if zero
```

So `eax` is given the contents of `rax` and then essentially ANDed with itself. If this results in zero (meaning the value in `eax` has to be zero), then we jump into the `Invalid login information.` section of the code.

To see if it's any clearer, I also used Ghidra to disassemble to C:

![C version of the code](disassemble1.png)

It seems that the variable I named `input_p` is initially created as an `int` pointer which points to 1 chunk of memory of 44 (0x2c) bytes in size.

Up to 20 (0x14) characters are then read in for the username using `fgets`. These are stored at location `input_p + 1`.

Another, up to, 20 characters are then read in for the password and stored at `input_p + 6`.

Then the choice of output depends on whether the contents of `input_p` is zero or not.

The behaviour of storing the username in `input_p + 1` and the password in `input_p + 6` seems odd, so I ran the code through the IDA Pro debugger to confirm my interpretation of the code. This is, in fact, what happens. 

As `input_p` is an int pointer (4 bytes per int), `input_p + 1` is actually 4 bytes on in memory, and `input_p + 6` is 24 bytes from the start of `input_p` in memory. The base address of `input_p` (in the current run) is 0x009A3010. If we look at this area of memory during a run we can see how that's setup:

![Memory dump](memory1.png)

As we approach the comparison, `rax` holds the address of `input_p`, and the statement:
```assembly
mov    eax,[rax]
```
moves the content of the address pointed to by `rax` into `eax`. As `eax` is a 32-bit register, that only reads the first four bytes, which are always zero.

It seems there is no exploit for `fgets()`, but there is some other information that may be useful. The address of the data is also stored on the stack (the local variable `input_p`):

![Memory dump](memory2.png)

If we could use the input to the username to manipulate this address (subtract 24 from it), then the password would end up being written starting at the original address rather than 24 bytes in as before. This would overwrite the first four bytes, later resulting in `eax` not being zero.

It turns out there may be a way to do this, as the code uses:
```c
fgets((char *)(input_p+1), x014, stdin);
printf("Enter password for user: ");
printf((char *)(input_p+1));
``` 

The fact the buffer is printed out with `printf()` and it's on it's own line means we could use the [format exploit](http://codearcana.com/posts/2013/05/02/introduction-to-format-string-exploits.html), but it needs some work to find out how.

First we need to find where the `input_p` variable sits on the stack in relation to the current stack pointer. I've found it difficult to see a relationship between what comes out when using the format exploit, and what values are on the stack. Without this it's difficult to know what offset is needed to change the `input_p` address.

I wrote a small program to try out various things with the format exploit until I understand what's actually happening. The program was:
```c
#include <stdio.h>

int main(int argc, char **argv)
{
  char buffer[100];
  fgets(buffer, 100, stdin);
  printf("You entered: ");
  printf(buffer);
  printf("\n");
  return 0;
}
```
and was compiled and run with:
```
> gcc -fno-stack-protector -o test_p test_p.c
test_p.c: In function ‘main’:
test_p.c:8:10: warning: format not a string literal and no format arguments [-Wformat-security]
   printf(buffer);
          ^
> python -c "print ('AAAA ' + '%x '*20)" | ./test_p 
You entered: AAAA 1293020 86cb0780 d 86ebe700 d 2b638938 0 41414141 25207825 20782520 78252078 25207825 20782520 78252078 25207825 a20 0 400650 400500 2b638930 
```
> Note: The `-fno-stack-protector` is so we can write to the stack without causing a problem. 

> Note also: We actually get a warning about the format exploit.

When we run it (passing in AAAA followed by 20 `%x` formatters (meaning "print hex values"), as there are no actual arguments to `printf()` in the code, it goes and gets its arguments from the stack to fill the formats. You can see that the 8th hex value returned is 0x41414141, which are the 4 'A's (0x41 in ASCII) we started the string with. This is followed by our %x characters, though they seem to be in an odd order and missing some bits. This is because `%x` is only showing the least-significant 32-bits. If we use `%p`, this shows the full 64-bit values and we see:
```
> python -c "print ('AAAA ' + '%p '*20)" | ./test_p 
You entered: AAAA 0x1621020 0x7f9a67ab9780 0xd 0x7f9a67cc7700 0xd 0x7ffde8020238 0x100000000 0x2070252041414141 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0xa20 (nil) 0x400650 0x400500 0x7ffde8020230
```
Here you can see the bytes are reversed - our 0x41414141 is displayed in the least-significant bytes of the 8th value, and it has the first part of the `%x`s in the most-significant bytes. This is clearer is we use ABCD instead of AAAA:
```
> python -c "print ('ABCD ' + '%p '*20)" | ./test_p 
You entered: ABCD 0x2514020 0x7f8e3e020780 0xd 0x7f8e3e22e700 0xd 0x7fff26cca578 0x100000000 0x2070252044434241 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0xa20 (nil) 0x400650 0x400500 0x7fff26cca570
```
Again, look at the 8th value and see that the 4 LSBs are 0x44434241.

I also tried to display just the ABCD (0x44434241) entry from the stack (8th position) with:
```
> ./test_p
ABCD %8$x
You entered: ABCD 44434241
> ./test_p
ABCD %8$p
You entered: ABCD 0x2438252044434241
```
That worked!

This still needs some further investigation: how we write to the stack using `%n` and to see what happens if the array is moved to the heap.

Once we find out how to access the appropriate stack position and write to it, we need to create something that can:
* Set up a bash socket to the server
* Read the initial input and extract the address (user = &lt;address&gt;)
* Subtract 24 (0x18) from the address
* Create a format string using some combination of %x, %p and %n to manupulate the `input_p` address to match the calculated one
* Write the constructed string to the socket (as username)
* Read the next line to consume it
* Write anything to the password
* Collect the flag!








