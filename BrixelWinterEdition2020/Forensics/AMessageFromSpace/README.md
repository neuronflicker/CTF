# A Message From Space
**Category:** [Forensics](../README.md)

**Points:** 10

**Description:**
I received a message from space

Beam me up scottie1!

**Files:** recording.wav

## Write-up
The *wav* file is a series of beeps and boops, like computer loading sounds.

As it was called 'A message from space' we wondered if this was a satellite transmission. We listened to a few *wav* files of actual satellite transmission, but none of them sounded right.

We then noticed that the description said *Beam me up scottie1!* and wondered why it said *scottie1* rather than just Scottie (or more correctly Scotty)? After a while searching Google, we found that Slow-Scan TV (SSTV), which is a method used by radio hams and the ISS to transmit pictures, has a format called scottie1.

We found some tools for converting SSTV signals to pictures, but some seemed to require playing the *wav* through a microphone so the program can listen to it. Luckily we found these instructions to play it through a virtual cable - [How to convert (decode) a Slow-Scan Television transmissions (SSTV) audio file](https://ourcodeworld.com/articles/read/956/how-to-convert-decode-a-slow-scan-television-transmissions-sstv-audio-file-to-images-using-qsstv-in-ubuntu-18-04). The instructions were for Ubuntu 18.04, but we followed them on Ubuntu 20.04 and it worked fine, with a couple of extra notes:
* Where it says use `sudo apt-get install hamlib-dev`, you should use `sudo apt-get install libhamlib-dev`.
* You have to click the *Start Receiver* (looks like a play button) in QSSTV to start receiving the played signal.

After following the instructions above, the image could be seen and it contained the flag.
