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
* [Section 6: Miscellaneous - Intro](#section-6-miscellaneous---intro)
* [Section 6: Miscellaneous - sudo](#section-6-miscellaneous---sudo)
* [Section 6: Miscellaneous - Adding users and groups](#section-6-miscellaneous---adding-users-and-groups)
* [Section 6: Miscellaneous - nano](#section-6-miscellaneous---nano)
* [Section 6: Miscellaneous - Basic shell scripting](#section-6-miscellaneous---basic-shell-scripting)
* [Section 6: Miscellaneous - Important Files and Directories](#section-6-miscellaneous---important-files-and-directories)
* [Section 6: Miscellaneous - Installing packages(apt)](#section-6-miscellaneous---installing-packagesapt)
* [Section 6: Miscellaneous - Processes](#section-6-miscellaneous---processes)
* [Fin ~](#fin-)
* [Bonus Challenge - The True Ending](#bonus-challenge---the-true-ending)

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
<password shown here>
shiba1@nootnoot:~$
```

### Questions
1. What's the password for shiba2
> &lt;password from above&gt;

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
<password shown here>
shiba2@nootnoot:~$
```

### Questions
1. What is shiba3's password
> &lt;password from above&gt;

## [Section 5: Advanced File Operations] - Intro
Explains what is in the next section - file permissions

No questions to answer - just click to confirm.

## [Section 5: Advanced File Operations] - A bit of background
Explains where to see file permissions with `ls -al`

No questions to answer - just click to confirm.

## [Section 5: Advanced File Operations] - chmod
Describes how to use `chmod` to change file permissions.

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

> *Author's note: The answer to question 1 isn't strictly correct. This also removes file recursively, rather than just in the directory. The answer should possibly be `rm *`, but using `rm` with either `*` or `-R` is dangerous and you should always make sure it's really what you want to do!*

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
```bash
shiba3@nootnoot:~$ ls -l /opt/secret/shiba4
-rwsrwxrwx 1 root root 8456 Feb 22  2020 /opt/secret/shiba4
shiba3@nootnoot:~$ ls -l /etc/shiba/shiba4
-rw-r--r-- 1 shiba4 shiba4 9 Feb 22  2020 /etc/shiba/shiba4
```
We can see that the `/opt/secret/shiba4` is executable, and `/etc/shiba/shiba4` is not, so `/opt/secret/shiba4` is probably what we want.

#### Create the test directory and test1234 file
> *Author's note: The machine seems to already have the `test` directory and `test1234` file, so I didn't need to create them. However, here's how you would do it if you need to*
```bash
shiba3@nootnoot:~$ mkdir test
shiba3@nootnoot:~$ touch test/test1234
shiba3@nootnoot:~$ ls
shiba_out.txt  test
shiba3@nootnoot:~$ ls test/
test1234
shiba3@nootnoot:~$
```

#### Run the executable
```bash
shiba3@nootnoot:~$ /opt/secret/shiba4
<password shown here>
shiba3@nootnoot:~$
```

### Questions
1. What is shiba4's password
> &lt;password from above&gt;

## [Section 6: Miscellaneous] - Intro
Intro to the miscellaneous commands section

No questions to answer - just click to confirm.

## [Section 6: Miscellaneous] - sudo
Explains how to use `sudo` to run commands as a root or other user.

### Questions
1. How do you specify which user you want to run a command as.
> -u

2. How would I run whoami as user jen?
> sudo -u jen whoami

3. How do you list your current sudo privileges(what commands you can run, who you can run them as etc.)
> -l

## [Section 6: Miscellaneous] - Adding users and groups
Describes how to create new users using `adduser` and groups using `addgroup`. It also explains the `usermod` command that enables you to add users to groups.

### Questions
1. How would I add the user test to the group test
> sudo usermod -a -G test test

## [Section 6: Miscellaneous] - nano
Gives a brief introduction to `nano` for creating/editing text files.

No questions to answer - just click to confirm.

## [Section 6: Miscellaneous] - Basic shell scripting
Describes how to create a shell script to run multiple commands.

No questions to answer - just click to confirm.

## [Section 6: Miscellaneous] - Important Files and Directories
This section describes a set of linux system files and directories, covering:
* /etc/passwd
* /etc/shadow
* /tmp
* /etc/sudoers
* /home
* /root
* /usr
* /bin and /sbin
* /var
* $PATH

No questions to answer - just click to confirm.

## [Section 6: Miscellaneous] - Installing packages(apt)
Describes how to use `apt` to install new packages (software) into Linux

No questions to answer - just click to confirm.

## [Section 6: Miscellaneous] - Processes
Describes how to use `ps` and `top` to look at running processes, and `kill` to stop one.

No questions to answer - just click to confirm.

## Fin ~
Closing statement

No questions to answer - just click to confirm.

## Bonus Challenge - The True Ending
Get the flag contained in `/root/root.txt`.

### Steps
To see the named file, we could log into the server with a user with `sudo` privileges. This may allow us to `cat` the *root.txt* file, but we may need to do more with permissions. Let's try this approach.

#### Log into the server
Deploy the machine, and then start Putty. 

We can log in as any user, as we're going to find out who has sudo, so just use *shiba1*.

Enter *shiba1@<IP-address-for-the-deployed-machine>* into the *Host Name* box and click *Open*.

When prompted for the password, enter *shiba1* (this was given in the text of [Section 1: SSH - Putty and SSH](#section-1-ssh---putty-and-ssh)).

Alternatively, use:
```
tc@hostcomputer:~$ ssh shiba1@<deployed-machine-IP>
shiba1@<deployed-machine-IP>'s password:
```

#### Check the file permissions
First we can check that a normal user can't just access the file - we don't want to waste time looking for a `sudo` user if we don't need one!
```
shiba1@nootnoot:~$ ls -la /root
ls: cannot open directory '/root': Permission denied
shiba1@nootnoot:~$ cat /root/root.txt
cat: /root/root.txt: Permission denied
shiba1@nootnoot:~$
```
It could be that one of the other *shiba* users has explicit permissions, but we may as well assume that they won't, and we need `sudo`

#### Find out which users have sudo
Although not given in the tasks, a quick [Google](https://www.google.com) (see [Google Dorking](https://tryhackme.com/room/googledorking) on [THM](https://tryhackme.com) for some advanced [Google](http://www.google.com) serching) shows there is an important file - `/etc/group` - that lists the members of groups. We can use this to find `sudo` members
```
shiba4@nootnoot:~$ grep sudo /etc/group
sudo:x:27:nootnoot
shiba1@nootnoot:~$
```
That was a little surprising - none of our `shiba` users have `sudo` permissions - only a user named `nootnoot` that we haven't come across before!

#### Find a password for the nootnoot user
We need to find a password for the nootnoot user so we can log in and use their `sudo` privileges.

First we can try with no password, just in case:
```
shiba1@nootnoot:~$ su - nootnoot
Password:
su: Authentication failure
shiba1@nootnoot:~$
```
No luck there. Let's have a look in the user's home directory:
```
shiba1@nootnoot:~$ ls -la /home/nootnoot/
total 40
drwxr-xr-x 5 nootnoot nootnoot 4096 Feb 22  2020 .
drwxr-xr-x 8 root     root     4096 Feb 22  2020 ..
-rw------- 1 nootnoot nootnoot 1113 Feb 22  2020 .bash_history
-rw-r--r-- 1 nootnoot nootnoot  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 nootnoot nootnoot 3771 Apr  4  2018 .bashrc
drwx------ 2 nootnoot nootnoot 4096 Feb 13  2020 .cache
drwx------ 3 nootnoot nootnoot 4096 Feb 13  2020 .gnupg
-rw-r--r-- 1 root     root     3893 Feb 22  2020 ll
drwxrwxr-x 3 nootnoot nootnoot 4096 Feb 22  2020 .local
-rw-r--r-- 1 nootnoot nootnoot  807 Apr  4  2018 .profile
-rw-r--r-- 1 nootnoot nootnoot    0 Feb 13  2020 .sudo_as_admin_successful
shiba1@nootnoot:~$
```
There doesn't seem much of interest here, but the `ll` file. let's take a look at that:
```
shiba1@nootnoot:~$ cat /home/nootnoot/ll
1
2
3
4
5
6
7
8
9
10
...
991
992
993
994
995
996
997
998
999
1000
shiba1@nootnoot:~$
```
This file just contains the numbers 1-1000, so no use to us.

In previous examples, we've had binaries to execute to find the next password. Let's see if there is an executable named `nootnoot` on the system:
```
shiba1@nootnoot:~$ find / * | grep nootnoot > nootnoot-out.txt
<lots of permission denied messages here>
shiba1@nootnoot:~$ cat nootnoot-out.txt
/home/shiba1/nootnoot-out.txt
/home/nootnoot/.sudo_as_admin_successful
/home/nootnoot/.profile
/home/nootnoot/.bashrc
/home/nootnoot/.bash_history
/home/nootnoot/.bash_logout
/home/nootnoot/ll
nootnoot-out.txt
shiba1@nootnoot:~$
```
So there don't seem to be any files with that name.

When we did [Binary - Shiba3](#binary---shiba3), along with the executable we wanted, there was a file named `/etc/shiba/shiba4`. When I looked in this file (and others in that directory) I found it contained the password for each user. We already know there isn't a file named *nootnoot*, so let's look for anything containing *nootnoot*. Let's start by searching only small files - searching every file would take a long time! We can use `find` to find only small files, and pass `grep` as an executable to it, according to the man page for `find` and some [Googling](https://www.google.com):
```
shiba1@nootnoot:~$ find / -type f -size 1k -exec grep -Il "nootnoot" {} \; > nootnoot-out.txt
<lots of permission denied messages here>
shiba1@nootnoot:~$ cat nootnoot-out.txt
/etc/hosts
/etc/hostname
/etc/subuid-
/etc/subgid-
/etc/group
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_dsa_key.pub
/etc/ssh/ssh_host_rsa_key.pub
/etc/group-
/etc/subgid
/etc/subuid
/run/cloud-init/instance-data.json
shiba1@nootnoot:~$
```
There's nothing of interest here. These are places where you'd expect *nootnoot* to exists, as it's also the hostname of the computer.

Permissions may differ for other users, so let's try our other *shiba* users and see if we get any different results.

We `su` as each user and run the above commands again.

We didn't have to look far - running the grep on small files for *shiba2* gave us a new file:
```
shiba2@nootnoot:~$ cat nootnoot-out.txt
/var/log/test1234
/etc/hosts
/etc/hostname
/etc/subuid-
/etc/subgid-
/etc/group
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_dsa_key.pub
/etc/ssh/ssh_host_rsa_key.pub
/etc/group-
/etc/subgid
/etc/subuid
/run/cloud-init/instance-data.json
shiba2@nootnoot:~$ cat /var/log/test1234
nootnoot:<nootnoot's password was here>
shiba2@nootnoot:~$
```

#### Log in as nootnoot and view the root file
```
shiba2@nootnoot:~$ su - nootnoot
Password:
nootnoot@nootnoot:~$ sudo cat /root/root.txt
[sudo] password for nootnoot:
<final flag was here>
nootnoot@nootnoot:~$
```

### Questions
1. Finish this room off! What is the root.txt flag
> &lt;flag from above&gt;

