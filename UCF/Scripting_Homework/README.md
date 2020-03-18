# Homework
**Category:** Scripting

**Points:** 50

**Description:**

Oops I forgot to do my math homework. Can you do it for me?

nc ctf.hackucf.org 10104

## Write-up
When you run the code on the server, it asks you to calculate a sum within 30 second:
```
> nc ctf.hackucf.org 10104
Hey, so I just realized my math homework is due in 30 seconds.
If you complete it for me, I'll give you a flag in return!
((224 % 159) + 68) % 204 = 
```
If you don't enter the answer within 30 seconds, the program times out.

We're going to need a script to read the sum, calculate it and send back the answer.

I started by isolating the sum from the rest of the text. First I set up a bash socket to get the first two lines from the server:
```bash
#!/bin/bash

# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10104

# Get the first two lines - the instructions
echo $(head -2 <&3)

# Use a random answer for now
echo 10 >&3

# Get any further data
cat <&3

# Close the socket
exec 3<&-
```
And that produced:
```
> ./homework.sh 
Hey, so I just realized my math homework is due in 30 seconds. If you complete it for me, I'll give you a flag in return!
0: Thanks to you I just failed my math exam. No flag for you!
```
The problem is that the sum we have to calculate is the prompt for the input. If I try to read the next line, the code will hang waiting for the answer. We have to read the sum one character at a time until we hit the `=` sign.
Also, we have to replace the `head` command from above because it reads everything up to the input in one command. Instead we use `read` to read a line at a time:
```bash
#!/bin/bash

# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10104

# Get the first two lines - the instructions
read -r nxt_line <&3
echo $nxt_line
read -r nxt_line <&3
echo $nxt_line

# Get the sum from the prompt - everything up to the '='
# Do it one character at a time so we don't hit the input request
# Start with an empty $sum string
sum=
while true
do
  read -r -n 1 nxt_c <&3

  if [ "$nxt_c" == "=" ]
  then
    break;
  fi

  # Add each character to teh end of $sum
  sum=$sum$nxt_c
done

echo Sum is: $sum

# Use a random answer for now
echo 10 >&3

# Get any further data
cat <&3

# Close the socket
exec 3<&-
```
Running this now give us:
```
./homework.sh 
Hey, so I just realized my math homework is due in 30 seconds.
If you complete it for me, I'll give you a flag in return!
Sum is: ((222/108)+253)%85
 0: Thanks to you I just failed my math exam. No flag for you
```
Now we add the calculation of the answer. We can use `bc` for that:
```bash
ans=$(echo "$sum" | bc)
echo Sum is: $sum = $ans

# Send the answer to the server
echo $ans >&3
```
Running with this code produces:
```
> ./homework.sh 
Hey, so I just realized my math homework is due in 30 seconds.
If you complete it for me, I'll give you a flag in return!
Sum is: ((224+209)*51)/149 = 148
 ((142 % 125) / 64) + 180 = 
```
Good. It looks like we got the right answer and are give a new some. Let's move the sum calculator into a function, and put a loop around it. The first time I tried with 100 iterations, but that still wanted more. Then I tried with 200 iterations, and the was perfect. The final script is:
```bash
#!/bin/bash

# Calculate the sum and send the answer
function calculate_sum
{
  # Do it one character at a time so we don't hit the input request
  # Get the sum from the prompt - everything up to the '='
  # Start with an empty $sum string
  sum=
  while true
  do
    read -r -n 1 nxt_c <&3
  
    if [ "$nxt_c" == "=" ]
    then
      break;
    fi
  
    # Add each character to teh end of $sum
    sum=$sum$nxt_c
  done

  ans=$(echo "$sum" | bc)
  echo Sum is: $sum = $ans

  # Send the answer to the server
  echo $ans >&3
}

# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10104

# Get the first two lines - the instructions
read -r nxt_line <&3
echo $nxt_line
read -r nxt_line <&3
echo $nxt_line

# Loop through the sums
for i in {1..200}
do
  calculate_sum
done

# Get any further data (hopefully including the flag)
cat <&3

# Close the socket
exec 3<&-
```

This worked and gave me the flag
