# Aunt Mildred
**Category:** RE

**Points:** 20

**Description:**

The flag is the password

> **Files:** mildred

## Write-up
As a matter of course, I always run the `strings` program over the executable to see if there's anything useful.

Part of the output was this:
```
> strings mildred
...
iD$$
D$,;D$ 
UWVS
[^_]
Usage: %s PASSWORD
malloc failed
ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ==
Correct password!
Come on, even my aunt Mildred got this one!
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
;*2$"8
GCC: (Ubuntu/Linaro 4.6.3-1ubuntu5) 4.6.3
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
...
```

The long string `ZjByX3...W5nNQ==` looks like it may be a base64 string, so I raan it through base64:
```
> echo ZjByX3kwdXJfNWVjMG5kX2xlNTVvbl91bmJhc2U2NF80bGxfN2gzXzdoMW5nNQ== | base64 -d 
```
This gave me the flag, but to double check, I ran it with the program:
```
> ./mildred [Insert flag here]
Correct password!
```
So it was the flag!
