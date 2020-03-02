# Conditional 1
**Category:** RE

**Points:** 5

**Description:**

Hint: If only there were a good tool to list strings used by a program...

> **Files:** conditional1

## Write-up
First I ran the downloaded executable:
```bash
> ./conditional1
Usage: ./conditional1 password
```
So we're looking for a password. The description gave a hint to the answer, so I tried:
```bash
> strings conditional1 | less
/lib/ld-linux.so.2
libc.so.6
_IO_stdin_used
puts
printf
memset
strcmp
__libc_start_main
/usr/local/lib:$ORIGIN
__gmon_start__
GLIBC_2.0
PTRh 
j3jA
[^_]
UWVS
t$,U
[^_]
Usage: %s password
super_secret_password
Access denied.
Access granted.
;*2$"(
...
```
The output show the string `super_secret_password` so I ran the program with this:
```bash
> ./conditional1 super_secret_password
Access granted.
```
This also displayed the flag
