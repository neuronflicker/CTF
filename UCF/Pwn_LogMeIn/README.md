# Log Me In!
<span style="color:red">Still in progress...</span>

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

Can we change the area written to in heap memory through input through `fgets`? Is there another way to exploit this? 

Other information that may or may not be useful. The address of the data is also stored on the stack:

![Memory dump](memory2.png)

If we could change the first byte from 0x10 to 0x06, this would cause data to be written in the first four bytes of the address.

Also, the code uses:
```c
fgets((char *)(input_p+1), x014, stdin);
printf("Enter password for user: ");
printf((char *)(input_p+1));
``` 

The fact the buffer is printed out with `printf()` and it's on it's own line means we could use the format exploit, but I need to work out how.

I think I should write a script to:
* Set up a bash socket to the server
* Read the first line (user = &lt;address&gt;)
* Strip out the address we need
* Use bash's printf to create a string
    * Use \x to specify components of the address we want to write to
    * Maybe use %x if necessary
    * Use %n to write to the address (any value but 0)
* Write the constructed string to the socket
* If we can't write to the heap address, see if we can locate and write to the place on the stack that the address is stored.







