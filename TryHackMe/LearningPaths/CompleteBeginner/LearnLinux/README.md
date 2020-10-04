# Learn Linux Room
Link: https://tryhackme.com/room/zthlinux

## Tasks
* [Intro](#intro)
* [Methodology](#methodology)
* [Section 1: SSH - Intro](#section-1-ssh---intro)
* [Section 1: SSH - Putty and SSH](#section-1-ssh---putty-and-ssh)
* [Section 2: Running Commands - Basic Command Execution](#section-2-running-commands---basic-command-execution)
* [Section 2: Running Commands - Manual Pages and Flags](#section-2-running-commands---manual-pages-and-flags)
* [Section 3: Basic File Operations - ls](#section-3-basic-file-operations---ls)
* [Section 3: Basic File Operations - cat](#section-3-basic-file-operations---cat)
* [Section 3: Basic File Operations - touch](#section-3-basic-file-operations---touch)
* [Section 3: Basic File Operations - Running a Binary](#section-3-basic-file-operations---running-a-binary)
* [Binary - Shiba1](#binary---shiba1)
* [su](#su)
* [Section 4: Linux Operators - Intro](#section-4-linux-operators-intro)
* [Section 4: Linux Operators - ">"](#section-4-linux-operators---)
* [Section 4: Linux Operators - ">>"](#section-4-linux-operators----1)
* [Section 4: Linux Operators - "&&"](#section-4-linux-operators----2)
* [Section 4: Linux Operators - "&"](#section-4-linux-operators----3)
* [Section 4: Linux Operators - "$"](#section-4-linux-operators----4)
* [Section 4: Linux Operators - "|"](#section-4-linux-operators----5)
* [Section 4: Linux Operators - ";"](#section-4-linux-operators----6)
* [Binary - shiba2](#binary---shiba2)
* [Section 5: Advanced File Operations - Intro](#section-5-advanced-file-operations---intro)
* [Section 5: Advanced File Operations - A bit of background](#section-5-advanced-file-operations---a-bit-of-background)
* [Section 5: Advanced File Operations - chmod](#section-5-advanced-file-operations---chmod)
* [Section 5: Advanced File Operations - chown](#section-5-advanced-file-operations---chown)
* [Section 5: Advanced File Operations - rm](#section-5-advanced-file-operations---rm)
* [Section 5: Advanced File Operations - mv](#section-5-advanced-file-operations---mv)
* [Section 5: Advanced File Operations - cp](#section-5-advanced-file-operations---cp)
* [Section 5: Advanced file Operations - cd && mkdir](#section-5-advanced-file-operations---cd--mkdir)
* [Section 5: Advanced File Operations - ln](#section-5-advanced-file-operations---ln)
* [Section 5 - Advanced File Operations - find](#section-5-advanced-file-operations---find)
* [Section 5: Advanced File Operations - grep](#section-5-advanced-file-operations---grep)
* [Binary - Shiba3](#binary---shiba3)

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
#### Create a text file called noot.txt:
```bash
shiba1@nootnoot:~$ touch noot.txt
shiba1@nootnoot:~$
```

#### Run the executable:
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
#### Create the 'test1234' environment variable and set it equal to '$USER'
```bash
shiba2@nootnoot:~$ export test1234=$USER
shiba2@nootnoot:~$ echo $test1234
shiba2
shiba2@nootnoot:~$
```

#### Run the 'shiba2' executable:
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

## [Section 5: Advanced File Operations] - A bit of background
Explains where to see file permissions with `ls -al`

No questions to answer - just click to confirm.

## [Section 5: Advanced File Operations] - chmod
Describes how to use `chmod` to change file permissions.

> *Author's note: Personally, I'm not keen on the "these numbers mean this, add numbers to get others" explanation as it's a little inconsistent (what do you add to get 4?). I prefer to think of the permissions as a 3-bit binary number (4's column as read, 2's column as write and 1's column as execute).*

### Questions
1. What permissions mean the user can read the file, the group can read and write to the file, and no one else can read, write or execute the file?
> 460

2. What permissions mean the user can read, write, and execute the file, the group can read, write, and execute the file, and everyone else can read, write, and execute the file.
> 777

## [Section 5: Advanced File Operations] - chown
Explains how to use the `chown` command to change the user and group for a file.

### Questions
1. How would you change the owner of file to paradox
> chown paradox file

2. What about the owner and the group of file to paradox
> chown paradox:paradox file

3. What flag allows you to operate on every file in the directory at once?
> -R

> *Author's note: The answer to question 3 isn't strictly correct, as it would change the permissions for all files within subsirectories, too. For 'every file in the directory' you should use `chown paradox *`*

## [Section 5: Advanced File Operations] - rm
Explains how to use `rm` to remove files

### Questions
1. What flag deletes every file in a directory
> -R

2. How do you suppress all warning prompts
> -f

> *Author's note: The answer to question 1 is very wrong. Using `-R` with `rm` is a very dangerous thing to do and you should know that this will recurse through subdirectories, deleting everything that matches! It's especially dangerous if you take into account question 2, and think you'll hide prompts - `rm -Rf` is one of the most dangerous combinations in Linux! The correct way to delete every file in the directory is to use `rm *`. or `rm -f *`*

## [Section 5: Advanced File Operations] - mv
Describes how files can be renamed and moved around using `mv`.

### Questions
1. How would you move file to /tmp
> mv file /tmp

## [Section 5: Advanced File Operations] - cp
Explains that `cp` will make a copy of a file.

No questions to answer - just click to confirm.

## [Section 5: Advanced file Operations] - cd && mkdir
Explains how to use `cd` to change to a different directory, and `mkdir` to create new directories.

### Questions
1. Using relative paths, how would you cd to your home directory.
> cd ~

2. Using absolute paths how would you make a directory called test in /tmp
> mkdir /tmp/test

## [Section 5: Advanced File Operations] - ln
Describe how `ln` can be used to create links to reference other files/directories. It also explains the difference between hard and symbolic links.

### Questions
1. How would I link /home/test/testfile to /tmp/test
> ln /home/test/testfile /tmp/test

## [Section 5 - Advanced File Operations] - find
Describes how to use `find` to locate files on the system.

### Questions
1. How do you find files that have specific permissions?
> -perm

2. How would you find all the files in /home
> find /home

3. How would you find all the files owned by paradox on the whole system
> find / -user paradox

## [Section 5: Advanced File Operations] - grep
Describes how to use `grep` to parse data for strings

### Questions
1. What flag lists line numbers for every string found?
> -n

2. How would I search for the string boop in the file aaaa in the directory /tmp
> grep boop /tmp/aaaa

## Binary - Shiba3
This is the next challenge. We need to run an executable named `shiba4` that will check for a directory named `test` in the user's home directory, that contains a file named `test1234`.

### Steps
#### Find the shiba4 executable
```bash
shiba3@nootnoot:~$ find / | grep shiba4 
```
This displayed a lot of 'Permission denied' lines, making it difficult to parse, so we can redirect the results to a file and just get the lines we want (Note: the 'Permission denied' messages will still be shown at the command line, but the file will just have the lines we want):
```bash
shiba3@nootnoot:~$ find / shiba4 | grep shiba4 > shiba_out.txt
find: ‘/var/log/unattended-upgrades’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/spool/rsyslog’: Permission denied
...
find: ‘/sys/fs/fuse/connections/48’: Permission denied
find: ‘/tmp/systemd-private-f2dd7578d9dd49fd96eb3ba555215565-systemd-resolved.service-Ax1YcC’: Permission denied
find: ‘/tmp/systemd-private-f2dd7578d9dd49fd96eb3ba555215565-systemd-timesyncd.service-4eRpAi’: Permission denied
shiba3@nootnoot:~$ cat shiba_out.txt
/opt/secret/shiba4
/home/shiba4
/home/shiba4/.profile
/home/shiba4/.bashrc
/home/shiba4/.bash_logout
/etc/shiba/shiba4
shiba3@nootnoot:~$
```
This shows two places of interest - `/opt/secret/shiba4` and `/etc/shiba/shiba4`. 

