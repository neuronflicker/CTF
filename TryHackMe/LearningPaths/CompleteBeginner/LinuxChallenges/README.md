# Linux Challenges Room
Link: https://tryhackme.com/room/linuxctf

## Tasks
* [Task 1: Linux Challenges Intro](#task-1-linux-challenges-intro)
* [Task 2: The Basics](#task-2-the-basics)
* [Task 3: Linux Functionality](#task-3-linux-functionality)

## Task 1: Linux Challenges Intro
Explains the purpose of this room, some of the commands and techniques you'll be expected to use, and how to deploy the machine

### Questions
1. Deploy the virtual machine.  
If you want to manually SSH into the machine, use the following credentials:  
Username: garry  
Password: letmein  
How many visible files can you see in garrys home directory?
> 3

> *Author's note: Initially I did `ls -la` which shows 7 files and 1 directory, but you should just use `ls` for this task.*

## Task 2: The Basics
This set of tasks will go over the basic linux commands.

Each question might require you to switch between another user to find the answer!

### Question 1
What is flag 1?

#### Steps
```
garry@ip-10-10-132-59:~$ cat flag1.txt
There are flags hidden around the file system, its your job to find them.

Flag 1: <flag 1 was here>

Log into bobs account to get flag 2.

Username: bob
Password: <bob's password was here>
garry@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 1 from above&gt;

### Question 2
Log into bob's account using the credentials shown in flag 1.

What is flag 2?

#### Steps
```
garry@ip-10-10-132-59:~$ su - bob
Password:
bob@ip-10-10-132-59:~$ ls
Desktop  Documents  Downloads  flag13  flag21.php  flag2.txt  flag8.tar.gz  Music  Pictures  Public  Templates  Videos
bob@ip-10-10-132-59:~$ cat flag2.txt
Flag 2: <flag 2 was here>
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 2 from above&gt;

### Question 3
Flag 3 is located where bob's bash history gets stored.

#### Steps
We can use `ls -a` to see what history files are available (ending with *_history*) and then `cat` those we see to find which contains the flag. As the shell is *bash*, it's likely to be *.bash_history*:
```
bob@ip-10-10-132-59:~$ ls -a
.              .cache     Downloads     .gconf         Music           Public            .viminfo     .xsession-errors
..             .config    flag13        .gnupg         .mysql_history  .selected_editor  .vnc
.bash_history  .dbus      flag21.php    .ICEauthority  .nano           .ssh              .Xauthority
.bash_logout   Desktop    flag2.txt     .local         Pictures        Templates         .Xclients
.bashrc        Documents  flag8.tar.gz  .mozilla       .profile        Videos            .xsession
bob@ip-10-10-132-59:~$ cat .bash_history
<flag 3 was here>
cat ~/.bash_history
rm ~/.bash_history
vim ~/.bash_history
exit
ls
crontab -e
ls
cd /home/alice/
ls
cd .ssh
ssh -i .ssh/id_rsa alice@localhost
exit
ls
cd ../alice/
cat .ssh/id_rsa
cat /home/alice/.ssh/id_rsa
exit
cat ~/.bash_history
exit
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 3 from above&gt;

### Question 4
Flag 4 is located where cron jobs are created.

#### Steps
*cron jobs* are stored in the *crontab*, and we can use the `crontab` command to edit or view them. We can list existing *cron jobs* with `crontab -l`:
```
bob@ip-10-10-132-59:~$ crontab -l
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command

0 6 * * * echo 'flag4:<flag 4 was here>' > /home/bob/flag4.txt
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 4 from above&gt;

### Question 5
Find and retrieve flag 5.

#### Steps
We can use `find` to locate any file named *flag5.txt*. So we don't have to search through lots of 'Permission denied' errors, we can send error output (stream number 2) to */dev/null*, which is equivalent to sending it nowhere (it just disappears).
```
bob@ip-10-10-132-59:~$ find / -name flag5.txt 2>/dev/null
/lib/terminfo/E/flag5.txt
bob@ip-10-10-132-59:~$ cat /lib/terminfo/E/flag5.txt
<flag 5 was here>
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 5 from above&gt;

### Question 6
"Grep" through flag 6 and find the flag. The first 2 characters of the flag is c9.

#### Steps
First we need to use `find` to locate *flag6.txt*, then we can use `grep` to locate the flag within the file.
<pre>
bob@ip-10-10-132-59:~$ find / -name flag6.txt 2>/dev/null
/home/flag6.txt
bob@ip-10-10-132-59:~$ grep c9 ../flag6.txt
Sed sollicitudin eros quis vulputate rutrum. Curabitur mauris elit, elementum quis sapien sed, ullamcorper pellentesque neque. Aliquam erat volutpat. Cras vehicula mauris vel lectus hendrerit, sed malesuada ipsum consectetur. Donec in enim id erat condimentum vestibulum <b>&lt;flag 6 was here&gt;</b> vitae eget nisi. Suspendisse eget commodo libero. Mauris eget gravida quam, a interdum orci. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Quisque eu nisi non ligula tempor efficitur. Etiam eleifend, odio vel bibendum mattis, purus metus consectetur turpis, eu dignissim elit nunc at tortor. Mauris sapien enim, elementum faucibus magna at, rutrum venenatis ipsum.
bob@ip-10-10-132-59:~$
</pre>
#### Answer
> &lt;flag 6 from above&gt;

### Question 7
Look at the systems processes. What is flag 7.

#### Steps
We can look at our own processes using `ps` to see if it's in a process we own, or everybody's processes with `ps -ef`.
```
bob@ip-10-10-132-59:~$ ps
  PID TTY          TIME CMD
 2494 pts/1    00:00:00 bash
 2602 pts/1    00:00:00 ps
bob@ip-10-10-132-59:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 22:23 ?        00:00:03 /sbin/init
root         2     0  0 22:23 ?        00:00:00 [kthreadd]
root         3     2  0 22:23 ?        00:00:00 [ksoftirqd/0]
root         5     2  0 22:23 ?        00:00:00 [kworker/0:0H]
...
root      1344     1  0 22:23 ?        00:00:00 /snap/amazon-ssm-agent/1068/amazon-ssm-agent
root      1357     1  0 22:23 ?        00:00:00 /usr/lib/policykit-1/polkitd --no-debug
mysql     1370     1  0 22:23 ?        00:00:01 /usr/sbin/mysqld
root      1390     1  0 22:23 ?        00:00:00 flag7:<flag 7 was here> 1000000
whoopsie  1391     1  0 22:23 ?        00:00:00 /usr/bin/whoopsie -f
root      1399     1  0 22:23 ?        00:00:00 /usr/sbin/sshd -D
root      1411     1  0 22:23 ?        00:00:00 /sbin/iscsid
root      1412     1  0 22:23 ?        00:00:00 /sbin/iscsid
...
bob       2603  2494  0 23:12 pts/1    00:00:00 ps -ef
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 7 was one of the process commands&gt;

### Question 8
De-compress and get flag 8.

#### Steps
Flag 8 is in *bob*'s home directory, as *flag8.tar.gz*. To get the contents we can use the `tar` command to decompress. The flag `-z` will unzip the *.gz* part, the flag `-x` will expand the *.tar* part, and the `-f` allows us to specify the file name to decompress.
```
bob@ip-10-10-132-59:~$ ls
Desktop    Downloads  flag21.php  flag5_find.txt  Music     Public     Videos
Documents  flag13     flag2.txt   flag8.tar.gz    Pictures  Templates
bob@ip-10-10-132-59:~$ tar -xzf flag8.tar.gz
bob@ip-10-10-132-59:~$ ls
Desktop    Downloads  flag21.php  flag5_find.txt  flag8.txt  Pictures  Templates
Documents  flag13     flag2.txt   flag8.tar.gz    Music      Public    Videos
bob@ip-10-10-132-59:~$ cat flag8.txt
<flag 8 was here>
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 8 from above&gt;

### Question 9
By look in your hosts file, locate and retrieve flag 9.

#### Steps
```
bob@ip-10-10-132-59:~$ cat /etc/hosts
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts

127.0.0.1       <flag 9 was here>.com
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 9 from above&gt;

### Question 10
Find all other users on the system. What is flag 10.

#### Steps
The best way to look at all the users on a Linux system is to look in the */etc/passwd* file.
```
bob@ip-10-10-132-59:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
syslog:x:104:108::/home/syslog:/bin/false
_apt:x:105:65534::/nonexistent:/bin/false
lxd:x:106:65534::/var/lib/lxd/:/bin/false
messagebus:x:107:111::/var/run/dbus:/bin/false
uuidd:x:108:112::/run/uuidd:/bin/false
dnsmasq:x:109:65534:dnsmasq,,,:/var/lib/misc:/bin/false
sshd:x:110:65534::/var/run/sshd:/usr/sbin/nologin
pollinate:x:111:1::/var/cache/pollinate:/bin/false
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
bob:x:1001:1001:Bob,,,:/home/bob:/bin/bash
<flag 10 was here>:x:1002:1002:,,,:/home/<flag 10 was here>:/bin/bash
alice:x:1003:1003:,,,:/home/alice:/bin/bash
mysql:x:112:117:MySQL Server,,,:/nonexistent:/bin/false
xrdp:x:113:118::/var/run/xrdp:/bin/false
whoopsie:x:114:120::/nonexistent:/bin/false
avahi:x:115:121:Avahi mDNS daemon,,,:/var/run/avahi-daemon:/bin/false
avahi-autoipd:x:116:122:Avahi autoip daemon,,,:/var/lib/avahi-autoipd:/bin/false
colord:x:117:125:colord colour management daemon,,,:/var/lib/colord:/bin/false
geoclue:x:118:126::/var/lib/geoclue:/bin/false
speech-dispatcher:x:119:29:Speech Dispatcher,,,:/var/run/speech-dispatcher:/bin/false
hplip:x:120:7:HPLIP system user,,,:/var/run/hplip:/bin/false
kernoops:x:121:65534:Kernel Oops Tracking Daemon,,,:/:/bin/false
pulse:x:122:127:PulseAudio daemon,,,:/var/run/pulse:/bin/false
rtkit:x:123:129:RealtimeKit,,,:/proc:/bin/false
saned:x:124:130::/var/lib/saned:/bin/false
usbmux:x:125:46:usbmux daemon,,,:/var/lib/usbmux:/bin/false
gdm:x:126:131:Gnome Display Manager:/var/lib/gdm3:/bin/false
garry:x:1004:1006:,,,:/home/garry:/bin/bash
bob@ip-10-10-132-59:~$
```
#### Answer
> &lt;flag 10 was a username&gt;

## Task 3: Linux Functionality
Now we have used the basic Linux commands to find the first 10 flags, we will move onto using more functions that Linux has to offer.

### Question 1
Run the command flag11. Locate where your command alias are stored and get flag 11.

#### Steps
We should run the `flag11` command and see what it does. Aliases in *bash* are usually created in the *~/.bashrc* file.
```
bob@ip-10-10-197-121:~$ flag11
You need to look where the alias are created...
bob@ip-10-10-197-121:~$ cat ~/.bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

<--snipped here-->

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

#custom alias
alias flag11='echo "You need to look where the alias are created..."' #<flag 11 was here>
...
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
bob@ip-10-10-197-121:~$
```
#### Answer
> &lt;flag 11 from above&gt;

### Question 2
Flag12 is located were MOTD's are usually found on an Ubuntu OS. What is flag12?

#### Steps
On Ubuntu, the Message of The Day (MOTD) is stored in */etc/update-motd.d/*, though it can sometimes be in */etc/motd* if there is a custom directory. These directories often store a set of files that go together to make the final message. Let's see what we have on the system:
```
ob@ip-10-10-197-121:~$ ls /etc/update-motd.d/
00-header     51-cloudguest         91-release-upgrade  98-fsck-at-reboot   99-esm
10-help-text  90-updates-available  97-overlayroot      98-reboot-required  logo.txt
bob@ip-10-10-197-121:~$ grep -i flag /etc/update-motd.d/*
/etc/update-motd.d/00-header:# Flag12: <flag 12 was here>
bob@ip-10-10-197-121:~$
```
#### Answer
> &lt;flag 12 from above&gt;

### Question 3
Find the difference between two script files to find flag 13.

#### Steps
There is a directory in the home directory that probably contains the script files we need to find the difference between. We can use `diff` to show the differences between two files.
```
bob@ip-10-10-197-121:~$ ls
Desktop  Documents  Downloads  flag13  flag21.php  flag2.txt  flag8.tar.gz  Music  Pictures  Public  Templates  Videos
bob@ip-10-10-197-121:~$ cd flag13/
bob@ip-10-10-197-121:~/flag13$ ls
script1  script2
bob@ip-10-10-197-121:~/flag13$ diff script1 script2
2437c2437
< Lightoller sees Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam...
---
> Lightoller sees <flag 13 was here> Smith walking stiffly toward him and quickly goes to him. He yells into the Captain's ear, through cupped hands, over the roar of the steam...
bob@ip-10-10-197-121:~/flag13$
```

#### Answer
> &lt;flag 13 from above&gt;

### Question 4
Where on the file system are logs typically stored? Find flag 14.

#### Steps
Logs on linux are stored in */var/log/*. Let's look in there and see if we can find the flag.
```
bob@ip-10-10-197-121:~/flag13$ ls /var/log/
alternatives.log    auth.log.2.gz          dpkg.log          hp             speech-dispatcher    wtmp.1
alternatives.log.1  btmp                   dpkg.log.1        kern.log       syslog               Xorg.0.log
amazon              btmp.1                 flagtourteen.txt  kern.log.1     syslog.1             Xorg.0.log.old
apache2             cloud-init.log         fontconfig.log    kern.log.2.gz  syslog.2.gz          xrdp-sesman.log
apt                 cloud-init-output.log  fsck              lastlog        syslog.3.gz
auth.log            cups                   gdm3              lxd            unattended-upgrades
auth.log.1          dist-upgrade           gpu-manager.log   mysql          wtmp
bob@ip-10-10-197-121:~/flag13$ cat /var/log/flagtourteen.txt
<-- snipped here -->
Boy desirous families prepared gay reserved add ecstatic say. Replied joy age visitor nothing cottage. Mrs door paid led loud sure easy read. Hastily at perhaps as neither or ye fertile tedious visitor. Use fine bed none call busy dull when. Quiet ought match my right by table means. Principles up do in me favourable affronting. Twenty mother denied effect we to do on.

Situation admitting promotion at or to perceived be. Mr acuteness we as estimable enjoyment up. An held late as felt know. Learn do allow solid to grave. Middleton suspicion age her attention. Chiefly several bed its wishing. Is so moments on chamber pressed to. Doubtful yet way properly answered humanity its desirous. Minuter believe service arrived civilly add all. Acuteness allowance an at eagerness favourite in extensive exquisite ye.

May indulgence difficulty ham can put especially. Bringing remember for supplied her why was confined. Middleton principle did she procuring extensive believing add. Weather adapted prepare oh is calling. These wrong of he which there smile to my front. He fruit oh enjoy it of whose table. Cultivated occasional old her unpleasing unpleasant. At as do be against pasture covered viewing started. Enjoyed me settled mr respect no spirits civilly.

Affronting everything discretion men now own did. Still round match we to. Frankness pronounce daughters remainder extensive has but. Happiness cordially one determine concluded fat. Plenty season beyond by hardly giving of. Consulted or acuteness dejection an smallness if. Outward general passage another as it. Very his are come man walk one next. Delighted prevailed supported too not remainder perpetual who furnished. Nay affronting bed projection compliment instrument.

<flag 14 was here>
bob@ip-10-10-197-121:~/flag13$
```
#### Answer
> &lt;flag 14 from above&gt;

### Question 5
Can you find information about the system, such as the kernel version etc.

Find flag 15.

#### Steps
First we can try `uname -r` which displays the kernel version. Other information is usually given with `uname -a`.
```
bob@ip-10-10-197-121:~$ uname -r
4.4.0-1075-aws
bob@ip-10-10-197-121:~$ uname -a
Linux ip-10-10-197-121 4.4.0-1075-aws #85-Ubuntu SMP Thu Jan 17 17:15:12 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
bob@ip-10-10-197-121:~$
```
Neither of these gave us anything that looked like a flag.

Maybe we need to look at the files in */proc* which is where this information is usually stored.
```
bob@ip-10-10-197-121:~$ grep -iI flag /proc/* 2>/dev/null
/proc/cpuinfo:flags             : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand hypervisor lahf_lm abm invpcid_single kaiser fsgsbase bmi1 avx2 smep bmi2 erms invpcid xsaveopt
/proc/kallsyms:0000000000000000 A xen_mc_irq_flags
/proc/kallsyms:0000000000000000 A nop_txn_flags
/proc/kallsyms:0000000000000000 T perf_misc_flags
/proc/kallsyms:0000000000000000 t __uncore_set_flag_sel_show
<-- snipped here -->
/proc/kallsyms:0000000000000000 t btrfs_ioctl_subvol_setflags   [btrfs]
/proc/kallsyms:0000000000000000 t btrfs_update_iflags   [btrfs]
/proc/kallsyms:0000000000000000 t btrfs_inherit_iflags  [btrfs]
/proc/kallsyms:0000000000000000 t reada_tree_block_flagged      [btrfs]
/proc/kallsyms:0000000000000000 t btrfs_set_disk_extent_flags   [btrfs]
bob@ip-10-10-197-121:~$
```
This gave lots of lines with *flag* in them, but none is what we want.

Another couple of places to look are */etc/os-release* and */etc/lsb-release* which have similar information to the previous locations.
```
bob@ip-10-10-197-121:~$ cat /etc/os-release
NAME="Ubuntu"
VERSION="16.04.5 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.5 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
bob@ip-10-10-197-121:~$ cat /etc/lsb-release
FLAG_15=<flag 15 was here>
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.5 LTS"
bob@ip-10-10-197-121:~$
```
Found it!

#### Answer
> &lt;flag 15 from above&gt;

### Question 6
Flag 16 lies within another system mount.

#### Steps
In Linux, other mounts are ususally found at */mnt* or */media*. Let's take a look.
```
bob@ip-10-10-197-121:~$ ls /mnt/
bob@ip-10-10-197-121:~$ ls /media/
f
bob@ip-10-10-197-121:~$ ls /media/f
l
bob@ip-10-10-197-121:~$ ls /media/f/l
a
bob@ip-10-10-197-121:~$ ls /media/f/l/a
g
bob@ip-10-10-197-121:~$ ls /media/f/l/a/g
1
bob@ip-10-10-197-121:~$ ls /media/f/l/a/g/1
6
bob@ip-10-10-197-121:~$ ls /media/f/l/a/g/1/6
is
bob@ip-10-10-197-121:~$ ls /media/f/l/a/g/1/6/is
<flag 16 was here>
bob@ip-10-10-197-121:~$
```
#### Answer
> &lt;flag 16 from above&gt;

### Question 7
Login to alice's account and get flag 17. Her password is TryHackMe123

#### Steps
```
bob@ip-10-10-197-121:~$ su - alice
Password:
alice@ip-10-10-197-121:~$ ls
flag17  flag19  flag20  flag22  flag23  flag32.mp3
alice@ip-10-10-197-121:~$ cat flag17
<flag 17 was here>
alice@ip-10-10-197-121:~$
```

#### Answer
> &lt;flag 17 from above&gt;

### Question 8
Find the hidden flag 18.

#### Steps
```
alice@ip-10-10-197-121:~$ ls -la
total 172
drwxr-xr-x 4 alice alice  4096 Feb 20  2019 .
drwxr-xr-x 6 root  root   4096 Feb 20  2019 ..
-rw------- 1 alice alice   518 Mar  7  2019 .bash_history
-rw-r--r-- 1 alice alice   220 Feb 18  2019 .bash_logout
-rw-r--r-- 1 alice alice  3771 Feb 18  2019 .bashrc
drwx------ 2 alice alice  4096 Feb 18  2019 .cache
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 flag17
-rw-rw-r-- 1 alice alice    33 Feb 18  2019 .flag18
-rw-rw-r-- 1 alice alice 99001 Feb 19  2019 flag19
-rw-rw-r-- 1 alice alice    45 Feb 19  2019 flag20
-rw-rw-r-- 1 alice alice    96 Feb 19  2019 flag22
-rw-rw-r-- 1 alice alice    33 Feb 19  2019 flag23
-rw-rw-r-- 1 alice alice 10560 Feb 19  2019 flag32.mp3
-rw------- 1 alice alice    32 Feb 19  2019 .lesshst
-rw-r--r-- 1 alice alice   655 Feb 18  2019 .profile
drw-r--r-- 2 alice alice  4096 Mar  7  2019 .ssh
-rw------- 1 alice alice  3075 Feb 19  2019 .viminfo
alice@ip-10-10-197-121:~$ cat .flag18
<flag 18 was here>
alice@ip-10-10-197-121:~$
```
#### Answer
> &lt;flag 18 from above&gt;

### Question 19
Read the 2345th line of the file that contains flag 19.

#### Steps
We can use the `sed` Stream EDitor command with the `-n` flag to suppress the output of every line not matching the pattern, with `'2345p'` being used to match and output the specified line number(s).
```
alice@ip-10-10-197-121:~$ sed -n '2345p' flag19
<flag 19 was here>
alice@ip-10-10-197-121:~$
```
#### Answer
> &lt;flag 19 from above&gt;
