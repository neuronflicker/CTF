# Tay
**Category:** Forensics

**Points:** 10

**Description:**

Author: kablaa

> **Files:** tay.jpg

## Write-up
When I first saw this, when I noticed it was a jpg, my first thought was steganography of some kind, so I Googled for some steganography tools.

I found `stegsolve`, with install instructions here: https://github.com/zardus/ctf-tools/blob/master/stegsolve/install
I also found `steghide`, which I just installed with:
```bash
> sudo apt-get install steghide
```

First I ran `stegsolve`, which enables you to view the image in different ways incase some text is hidden in a plane of the image (I took instructions from [here](https://www.youtube.com/watch?v=9-YczGtaIiY)):
```bash
> java -jar stegsolve.jar
```
I cycled through the different image planes/colourmaps, but this gave nothing.

Next I tried running `steghide`. It asked for a passphrase, so I tried an empty passphrase, I tried ''tay'' and ''Tay''. They all gave the same result:
```bash
> steghide extract -sf tay.jpg 
Enter passphrase: 
steghide: could not extract any data with that passphrase!
```

I thought maybe there was a clue in the binary of the image, so I ran `strings` over it to see if there was anything there. At the end there was an unusually long string:
```bash
> strings tay.jpg
...
_dk[
8s2,
u))g
qrcz
ZmxhZ3tuNHoxX3MzeC1yMGIwN30=
```

As this string ended with an equals sign, I made a guess that it was base64 (as they use '=' for padding characters). I decoded it with:
```bash
> echo ZmxhZ3tuNHoxX3MzeC1yMGIwT30= | base64 -d
```

Luckily, this worked, and gave me the flag.
