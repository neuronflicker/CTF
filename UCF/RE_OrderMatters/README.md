# Order Matters
**Category:** RE

**Points:** 75

**Description:**

The vault to access this armory is protected with some weird password algorithm. Can you gain access?

Author: Winyl

Hint: The password should be a string of unique numbers (eq. 1 2 11 12 = 01021112)

> **Files:** order

## Write-up
When run, the code asks for a password. The hint in the description suggests that you need to enter a series of 2-digit numbers for the password. I ran the code and tried entering the value from the hint:
```
> ./order 
Enter password: 01021112
Wrong password length.
```
Now we need to look at the code to find out how long this password needs to be. Decompiling the binary in Ghidra, we can see a `main()` function, and it contains this section of code:
```c
  printf("Enter password: ");
  __isoc99_scanf(&DAT_00100d4c,passwd);
  passwd_len = strlen(passwd);
  if (passwd_len != 0x1e) {
    puts("Wrong password length.");
                    /* WARNING: Subroutine does not return */
    exit(-1);
  }
```
The password length is tested against the value `0x1e`, which is 30 in decimal. Therefore, we need 30 characters in the password to get past this section. I tested with 30 digits:
```
> ./order 
Enter password: 123456789012345678901234567890
Access Denied.
```
So we can move forward by passing 30 digits. As the hint suggests the password consists of 2-digit numbers, that means we're going to end up with 15 numbers making up the password. The hint also says the numbers are unique, so we can try passing 15 unique numbers to the password:
```
> ./order 
Enter password: 010203040506070809101112131415
Access Denied.
```
OK. So now we need to find out what the actual numbers we need are. Let's look a little further into the `main()` function in the Ghidra decompile:
```c
  local_c = 0;
  while (local_c < 0xf) {
    *(int *)((long)&local_78 + (long)local_c * 4) =
         *(int *)((long)&local_78 + (long)local_c * 4) + ((int)passwd[local_10] + -0x30) * 10;
    *(int *)((long)&local_78 + (long)local_c * 4) =
         *(int *)((long)&local_78 + (long)local_c * 4) + (int)passwd[local_10 + 1] + -0x30;
    local_10 = local_10 + 2;
    local_c = local_c + 1;
  }
```
This section of code seems to convert the numbers entered by the user into an array of `int`. In the Ghidra decompile, the 'array' is shown as a series of 8-byte variables and a 4-byte variable:
```c
  undefined8 local_78;
  undefined8 local_70;
  undefined8 local_68;
  undefined8 local_60;
  undefined8 local_58;
  undefined8 local_50;
  undefined8 local_48;
  undefined4 local_40;
```
However, the conversion code above shows we write to each 4-byte part of these variables (`local_c * 4` is used for addressing and added to `local_78` - the start of the 'array'). Each character of the passwd is accessed with `passwd[local_10]` and `passwd[local_10 + 1]`, the first accessing the tens column of the number, and the second the units. Each character then has `0x30` subtracted from it. This is the ASCII value for the '0' character. The while loop runs while `local_c < 0xf`. As it starts at 0, this will process our 15 2-digit values. Therefore, when this section of code is finished, all of the entered password values will be in the 'array' as `int` values.

Now we can look at the rest of the `main()` function which interprets our password and tests it's valid:
```c
  local_c = 0;
  while (local_c < 0xf) {
    switch(*(undefined4 *)((long)&local_78 + (long)local_c * 4)) {
    case 1:
      iVar1 = p01();
      local_14 = local_14 + iVar1;
      break;
    case 2:
      iVar1 = p02();
      local_14 = local_14 - iVar1;
      break;
    case 3:
      iVar1 = p03();
      local_14 = local_14 * iVar1;
      break;
    case 4:
      iVar1 = p04();
      local_14 = local_14 + iVar1;
      break;
    case 5:
      iVar1 = p05();
      local_14 = local_14 - iVar1;
      break;
    case 6:
      iVar1 = p06();
      local_14 = local_14 * iVar1;
      break;
    case 7:
      iVar1 = p07();
      local_14 = local_14 + iVar1;
      break;
    case 8:
      iVar1 = p08();
      local_14 = local_14 - iVar1;
      break;
    case 9:
      iVar1 = p09();
      local_14 = local_14 * iVar1;
      break;
    case 10:
      iVar1 = p10();
      local_14 = local_14 + iVar1;
      break;
    case 0xb:
      iVar1 = p11();
      local_14 = local_14 - iVar1;
      break;
    case 0xc:
      iVar1 = p12();
      local_14 = local_14 * iVar1;
      break;
    case 0xd:
      iVar1 = p13();
      local_14 = local_14 + iVar1;
      break;
    case 0xe:
      iVar1 = p14();
      local_14 = local_14 - iVar1;
      break;
    case 0xf:
      iVar1 = p15();
      local_14 = local_14 * iVar1;
    }
    local_c = local_c + 1;
  }
  if (((int)local_14 >> 0x1f ^ local_14) - ((int)local_14 >> 0x1f) == 0x6f2e255a) {
    puts("Access Granted");
  }
  else {
    puts("Access Denied.");
  }
  return;
```
This loops through our 'array' (15 `int` values) and checks each value in a `switch` statement. A variable `local_14` is calculated as we go, and checked against a set value (`0x6f2e255a`) with some shifts, an XOR and a subtraction to calculate the final value.

From the `switch` statement we can determine what the unique numbers in the password should be, as we can see each comparison. Here we can see each value is compared to a number from 1 to 0xf (01 to 15). These numbers must be what makes up our password. As we know they should be unique, we know we have to use each of the numbers, and, as we tried them in the correct order earlier, and that didn't work, we also know we have to find the correct order (hence the name of the CTF!).

Looking at the `switch` we can see that the `local_14` is updated with each `case` statement. A different function is called (`p1()` to `p15()`) in each `case`, and the return value of that function is applied to the `local_14` variable through various mathematical operations (the different mathematicall operators mean the result will differ if the are not applied in the correct order). Hopefully, the functions will give us some sort of clue as to the order we need to apply them - trying to work backwards from the expected result will be difficult!

So each of the `p1()` to `p15()` functions are very similar:
```c
void p01(void)
{
  strtol("58335249",(char **)0x0,0x10);
  return;
}
```
The only difference between the functions is the value in the string. The `strol()` function converts a numerical string to a long integer. The final parameter is the base the number is in. Here, the base is 16 (`0x10`), so we know these values are hexadecimal.

Here is a list of all the values:

| p number | Hex Value | Dec Value |
|:--------:|:---------:|----------:|
|   01     |  58335249 | 1,479,758,409 |
|   02     |  58306c45 | 1,479,568,453 |
|   03     |  5a314e66 | 1,513,180,774 |
|   04     |  63335675 | 1,664,308,853‬ |
|   05     |  58335177 | 1,479,758,199 |
|   06     |  51563969 | 1,364,605,289‬ |
|   07     |  4e484a45 | 1,313,360,453 |
|   08     |  66513d3d | 1,716,600,125 |
|   09     |  4d313935 | 1,295,071,541 |
|   10     |  59544578 | 1,498,695,032 |
|   11     |  4d313943 | 1,295,071,555 |
|   12     |  4d486c7a | 1,296,591,994‬ |
|   13     |  6532315a | 1,697,788,250‬ |
|   14     |  5831526f | 1,479,627,375 |
|   15     |  556a4675 | 1,433,028,213 |

OK. So let's try a simple ordering - numerical order ascending. No values are the same, so there's no ambiguity:
```
> ./order
Enter password: 091107061502140501100304081213
Access Denied.
```
I also tried the reverse:
```
> ./order
Enter password: 131208040310010514021506071109
Access Denied.
```
After staring at this for a while, I notice that the numbers (if we take each to be 4 chars long) range from `0x30` to `0x7a`, which mean they all fit in the printable ASCII range. The above table is extended with the ASCII characters for the hex strings:
| p number | Hex Value | Dec Value | ASCI Conversion |
|:--------:|:---------:|----------:|:---------------:|
|   01     |  58335249 | 1,479,758,409 | X3RI
|   02     |  58306c45 | 1,479,568,453 | X0lE
|   03     |  5a314e66 | 1,513,180,774 | Z1Nf
|   04     |  63335675 | 1,664,308,853‬ | c3Vu
|   05     |  58335177 | 1,479,758,199 | X3Qw
|   06     |  51563969 | 1,364,605,289‬ | QV9i
|   07     |  4e484a45 | 1,313,360,453 | NHJE
|   08     |  66513d3d | 1,716,600,125 | fQ==
|   09     |  4d313935 | 1,295,071,541 | M195
|   10     |  59544578 | 1,498,695,032 | YTEx
|   11     |  4d313943 | 1,295,071,555 | M19C
|   12     |  4d486c7a | 1,296,591,994‬ | MHlz
|   13     |  6532315a | 1,697,788,250‬ | e21Z
|   14     |  5831526f | 1,479,627,375 | X1Ro
|   15     |  556a4675 | 1,433,028,213 | UjFu

There is no obvious pattern to these ASCII conversions, but it should be noted that all characters are alpha-numeric, the only exceptions being the two equals signs. This suggests that these strings could be base64 encoded.

Let's extend the table again with the base64 decoded versions of these strings. I did this in Linux using the line:
```
> echo X3RI | base64 --decode
_tH
```
I did this for every line and completed the table:
| p number | Hex Value | Dec Value | ASCI Conversion | Base64 decoded |
|:--------:|:---------:|----------:|:---------------:|:--------------:|
|   01     |  58335249 | 1,479,758,409 | X3RI | _tH
|   02     |  58306c45 | 1,479,568,453 | X0lE | _ID
|   03     |  5a314e66 | 1,513,180,774 | Z1Nf | gS_
|   04     |  63335675 | 1,664,308,853‬ | c3Vu | sun
|   05     |  58335177 | 1,479,758,199 | X3Qw | _t0
|   06     |  51563969 | 1,364,605,289‬ | QV9i | A_b
|   07     |  4e484a45 | 1,313,360,453 | NHJE | 4rD
|   08     |  66513d3d | 1,716,600,125 | fQ== | }
|   09     |  4d313935 | 1,295,071,541 | M195 | 3_y
|   10     |  59544578 | 1,498,695,032 | YTEx | a11
|   11     |  4d313943 | 1,295,071,555 | M19C | 3_B
|   12     |  4d486c7a | 1,296,591,994‬ | MHlz | 0ys
|   13     |  6532315a | 1,697,788,250‬ | e21Z | {mY
|   14     |  5831526f | 1,479,627,375 | X1Ro | _Th
|   15     |  556a4675 | 1,433,028,213 | UjFu | R1n

This looks promising. The braces suggest this might be the flag scrambled up. If we can order this and get a flag, we can then test it by entering the relevant numbers as the password, and hopefully get an `Access Granted` message!

Looking on the UCF website, the information about this challenge shows that it was part of sunshinectf-2018. The flags for these begin with `sun`, so we can assume this one does, too. As a flag then uses an open brace, and ends with a close brace, we can assume the first two numbers and the last:
```
sun{mY...}
0413...08
```
This is now just a case of trying some rearrangements of the parts of the flags until we get the final answer. This took a bit of time to do, but I finally got there:
```
> ./order
Enter password: 041302061503101411120501090708
Access Granted
```
The flag was given by the final combination