# Repetition 1
**Category:** Scripting

**Points:** 10

**Description:**

Simon says repeat after me

nc ctf.hackucf.org 10101

## Write-up
Running the code on the server sets up a repetition challenge:
```bash
> nc ctf.hackucf.org 10101
This will be fun, repeat after me!
Better be quick, you only have 30 seconds!
Value: 2790848092
Repeat: 2790848092
Value: 52174777
Repeat: 52174777
Value: 4177833735
```
There isn't enough time to fill the repeats by hand, so a script is needed to read the prompts in and write the answers out.

At first I just tried to write this in a naive way - just trying to send 1 to the input:
```bash
#!/bin/bash
do_rep=0
while IFS='$\n' read -r line
do
  if [ $do_rep -eq 1 ]
  then
    echo -e "1\n"
    do_rep=0
  fi
  echo Next: $line
  case "$line" in
    Value* ) do_rep=1;;
  esac
done
```

This just paused at the input stage waiting for my input before starting the next iteration of the loop contents. 

After some Googling I came across `expect`, installed it and tried using that:
```bash
#!/usr/bin/expect -f

spawn nc ctf.hackucf.org 10101
expect "Repeat:" { send "1\r" }
expect eof
```

That kind of worked:
```
> ./reply.sh
spawn nc ctf.hackucf.org 10101
This will be fun, repeat after me!
Better be quick, you only have 30 seconds!
Value: 1267350308
Repeat: 1
You have a very bad memory if you've already forgotten that number.
```
but I was unsure what string manipulation I could do in `expect`.

Instead, I found [this page](https://n0where.net/bash-open-tcpudp-sockets) about TCP sockets in bash, so I thought I'd give them a try. First just sending 1 to the first reply:
```bash
#!/bin/bash

# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10101

# Get input from the socket and display
cat <&3

# Send output to the socket
echo "1" >&3

# Check for further output
cat <&3

# Close the socket
exec 3<&-
```

This didn't work - it just hung at the user input. After some further searches, I found that `cat` will try to get all data in one go, but we can use `head` instead to just pull in a set number of lines:
```bash
#!/bin/bash

# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10101

# Get input from the socket and display
# for the header we just want 3 lines
head -3 <&3

# Send output to the socket
echo "1" >&3

# Check for further output
cat <&3

# Close the socket
exec 3<&-
```

This did better, and actually sent the number to the server:
```
>  ./repeat.sh 
This will be fun, repeat after me!
Better be quick, you only have 30 seconds!
Value: 1511294255
You have a very bad memory if you've already forgotten that number.
```

Now we can grab the actual value from the line and send it:
```bash
#!/bin/bash

# Pass in the input, calculate the output and send
function get_value_and_send()
{
  # Sometimes there are 2 colons, and sometimes 1
  # So use this to get whatever is after the last value after a colon 
  echo $1
  val=${1##*:}
  echo Sending: $val
  echo $val >&3

}


# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10101

# Get input from the socket and display
# Read the three header lines (includes first value)
inp=$(head -3 <&3)
get_value_and_send "$inp"

# After some test runs I find it runs 100 times.
# We've done the first one above, so do the rest...
for i in {1..99}
do
  # Only get one line at a time now
  inp=$(head -1 <&3)
  get_value_and_send "$inp"
done

# Check for further output
# Hopefully including the flag
cat <&3

# Close the socket
exec 3<&-
```

> Note: I ran it with a count a couple of times to check the number of values needed

This worked, and gave me the flag.
