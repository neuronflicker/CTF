# Useful notes for Pwn challenges

- [scanf() and read()](#scanf-and-read)

# scanf() and read()
`scanf()` and `read()` both read in data from the user. While `scanf()` will read input characters until it hits a whitespace character, `read()` will read characters (whatever they are) until it has read the number of characters specified in the function.

Therefore, the following code:
```c
// my_prog.c
int main()
{
  char fname[25];
  char sname[25];

  scanf("%s", fname);
  scanf("%s", sname);

  return 0;
}
```
The values can be easily read in from a file:
```
fred
dref
```
The buffers can also be overflowed by passing more than 25 characters to it (`scanf()` will keep on reading!).

However, with `read()`:
```c
// my_prog.c
int main()
{
  char fname[25];
  char sname[25];

  read(STDIN_FILENO, fname, sizeof(fname));
  read(STDIN_FILENO, sname, sizeof(fname));

  return 0;
}
```
This is fine if reading from the command line, but reading from the file, `fname` will end up with something like `fred\ndref` in it. To get past these `read()` calls from a file you have to include 25 characters for each.

This is a problem when using [IDA](ida.md), because it has to take input from a file into the debugger - it doesn't seem to prompt for user input. 