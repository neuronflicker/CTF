# Repetition 2
**Category:** Scripting

**Points:** 10

**Description:**

Simon is mean

nc ctf.hackucf.org 10102

## Write-up
I started this challenge with the script from [Repetition1](../Scripting_Repetition1/README.md), as the initial prompts were the same. I just edited the port number: 
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
exec 3<>/dev/tcp/ctf.hackucf.org/10102

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

This still worked, but now we have an extra prompt at the end:
```bash
...
Sending: 3894816467
Value: 1572959184
Sending: 1572959184
Repeat: Good job!
Now, I hope you were paying attention...
What was the first value I told you?
First:
```

As the script I have gets the first value outside the loop, this answer can be stored easily:
```bash
# Initialise a bash socket using FID 3 as tcp with input and output
exec 3<>/dev/tcp/ctf.hackucf.org/10102

# Get input from the socket and display
# Read the three header lines (includes first value)
inp=$(head -3 <&3)
get_value_and_send "$inp"

# Though val is in a function, unless otherwise specified, it is global
first=$val
```

Now we can use this, and the example output we've seen, to send the value to first after our loop:
```bash
# Deal with the request for the first value
# Receive three lines, then send $first
head -3 <&3
echo $first >&3

# Check for further output
# Hopefully including the flag
cat <&3

# Close the socket
exec 3<&-
```

Running this script worked, and gave me the flag.
