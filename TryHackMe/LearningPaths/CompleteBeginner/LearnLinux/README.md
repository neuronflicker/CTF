# Learn Linux Room
Link: https://tryhackme.com/room/zthlinux

## Tasks
* [Intro](#intro)
* [Methodology](#methodology)
* [Section 1: SSH - Intro](#section-1:-ssh---intro)

## Intro
No questions to answer - just click to confirm and deploy the machine

## Methodology
Description of how this room works.

No questions to answer - just click to confirm.

## [Section 1: SSH] - Intro
A brief intro to what SSH is.

No questions to answer - just click to confirm.

## [Section 1: SSH] - Putty and ssh
Describes how to use Putty or a command line ssh to access the Linux machine.

No questions to answer - just follow the instructions and click to confirm.

## [Section 2: Running Commands] - Basic Command Execution
Explains how to run a command (in this case `echo`) from the SSH shell

No questions to answer - just follow the instructions and click to confirm.

## [Section 2: Running Commands] - Manual Pages and Flags
Describes how to access the man pages and apply flags/switches to a command

### Questions
1. How would you output hello without a newline
> echo -n hello

## [Section 3: Basic File Operations] - ls
Describes how to use `ls` to list files in a directory

### Questions
1. What flag outputs all entries
> -a

2. What flag outputs things in a "long list" format 
> -l

## [Section 3: Basic File Operations] - cat
Explains the `cat` command for outputting the contents of files to the console

### Questions
1. What flag numbers all output lines?
> -n

## [Section 3: Basic File Operations] - touch
Describes the `touch` command when used for creating files.

No questions to answer - just click to confirm.

## [Section 3: Basic File Operations] - Running a Binary
Explains relative paths and how to run downloaded/created binaries

### Questions
1. How would you run a binary called hello using the directory shortcut . ?
> ./hello

2. How would you run a binary called hello in your home directory using the shortcut ~ ?
> ~/hello

3. How would you run a binary called hello in the previous directory using the shortcut .. ?
> ../hello

## Binary - Shiba1
This is the first challenge - run a binary to get the password for *shiba2*

### Steps
1. Create a text file called noot.txt:
```bash
shiba1@nootnoot:~$ touch noot.txt
shiba1@nootnoot:~$
```

2. Run the executable:
```bash
shiba1@nootnoot:~$ ./shiba1
pinguftw
shiba1@nootnoot:~$
```

### Questions
1. What's the password for shiba2
> pinguftw

## su
Describes how to use `su` to change users. We use the above password to log in as *shiba2*

Use this to switch to the *shiba2* user for the following tasks. 

I also changed to the home directory of *shiba2* otherwise you don't have permissions for subsequent tasks. This can be done by using the `cd` command after switching users:
```bash
shiba1@nootnoot:~$ su shiba2
Password:
shiba2@nootnoot:/home/shiba1$ cd ~
shiba2@nootnoot:~$
```
or by using the `su` command by supplying the `-` flag:
```bash
shiba1@nootnoot:~$ su - shiba2
Password:
shiba2@nootnoot:~$
```

### Questions
1. How do you specify which shell is used when you login?
> -s

## [Section 4: Linux Operators] - Intro
Introduction to command line operators

No questions to answer - just click to confirm.

## [Section 4: Linux Operators] - ">"
Describes the redirection operator, `>`, and how to redirect the output of a command to a file

### Questions
1. How would you output twenty to a file called test
> echo twenty > test

## [Section 4: Linux Operators] - ">>"
Describes how to append text to a file in Linux, rather than erasing any existing contets as happened with `>`

No questions to answer - just click to confirm.

## [Section 4: Linux Operators] - "&&"
Describes how to run a second command if the first is successful

No questions to answer - just click to confirm.

## [Section 4: Linux Operators] - "&"
Explains how `&` can run a command that takes a long time in the background, allowing you to still interact with the shell.

No questions to answer - just click to confirm.

## [Section 4: Linux Operators] - "$"
Describes how to set environment variables and then reference them using `$`.

### Questions
1. How would you set nootnoot equal to 1111
> export nootnoot=1111

2. What is the value of the home environment variable
> /home/shiba2

## [Section 4: Linux Operators] - "|"
Explains the use of the pipe operator (`|`) to pass the output from one command into the input of another.

No questions to answer - just click to confirm.

## [Section 4: Linux Operators] - ";"
Explains the use of the semicolon operator (`;`) to run two commands in a single line.

No questions to answer - just click to confirm.

## Binary - shiba2
This is the next challenge, running a binary that checks that the environment variable `test1234` is set to the same as `$USER`.

The output if successful is the password for *shiba3*.

### Steps
1. Create the `test1234` environment variable and set it equal to `$USER`
```bash
shiba2@nootnoot:~$ export test1234=$USER
shiba2@nootnoot:~$ echo $test1234
shiba2
shiba2@nootnoot:~$
```

2. Run the `shiba2` executable:
```bash
shiba2@nootnoot:~$ ./shiba2
happynootnoises
shiba2@nootnoot:~$
```

### Questions
1. What is shiba3's password
> happynootnoises

## [Section 5: Advanced File Operations] - Intro
Explains what is in the next section - file permissions

No questions to answer - just click to confirm.

