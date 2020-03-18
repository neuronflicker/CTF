# rabbit-hole
**Category:** Forensics

**Points:** 30

**Description:**

Author: wil0la

> **Files:** rabbit-hole.png

## Write-up
The downloaded file is an image of Alice playing croquet:

![The image](rabbit-hole.png)

I first ran `strings` on the image to see what it brought up, but there was nothing obvious.

Next I used `stegsolve` to look at the image, but it didn't help, either.

Because the image was a `png` file, I ran `pngcheck` to see if that gave me any information. It did:
```
> pngcheck -v rabbit-hole.png 
File: rabbit-hole.png (69202 bytes)
  chunk IHDR at offset 0x0000c, length 13
    473 x 689 image, 4-bit palette, non-interlaced
  chunk pHYs at offset 0x00025, length 9: 31496x31496 pixels/meter (800 dpi)
  chunk PLTE at offset 0x0003a, length 48: 16 palette entries
  chunk tEXt at offset 0x00076, length 163:   text has control character(s) (13) (discouraged)
, keyword: Comment
  chunk IDAT at offset 0x00125, length 67772
    zlib: deflated, 32K window, default compression
  chunk IEND at offset 0x109ed, length 0
  additional data after IEND chunk
ERRORS DETECTED in rabbit-hole.png
```
This suggests there is someting included in the image after the `IEND` chunk. I had a look at the image in a hex viewer (`HxD` on Windows) and I could see the name `data.txt`. It can be seen in the end of a `hexdump`:
```
> hexdump -C rabbit-hole.png | tail -10
00010dd0  5d d4 0d c2 70 24 f7 c9  f8 69 47 12 79 d1 36 c4  |]...p$...iG.y.6.|
00010de0  f2 8b c2 eb fa e3 60 a8  ff bf 76 55 f8 17 50 4b  |......`...vU..PK|
00010df0  01 02 1e 03 14 00 00 00  08 00 80 a4 79 48 0c 71  |............yH.q|
00010e00  ca f9 b7 03 00 00 7f 06  00 00 08 00 18 00 00 00  |................|
00010e10  00 00 01 00 00 00 a4 81  00 00 00 00 64 61 74 61  |............data|
00010e20  2e 74 78 74 55 54 05 00  03 30 a1 f5 56 75 78 0b  |.txtUT...0..Vux.|
00010e30  00 01 04 e8 03 00 00 04  e8 03 00 00 50 4b 05 06  |............PK..|
00010e40  00 00 00 00 01 00 01 00  4e 00 00 00 f9 03 00 00  |........N.......|
*
00010e52
```
I used `binwalk` to try to extract any files in there:
```
> binwalk -e rabbit-hole.png 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 473 x 689, 4-bit colormap, non-interlaced
297           0x129           Zlib compressed data, default compression
68085         0x109F5         Zip archive data, at least v2.0 to extract, compressed size: 951, uncompressed size: 1663, name: data.txt
69180         0x10E3C         End of Zip archive
```
This gave me the directory `_rabbit-hole.png.extracted` which contained:
```
> ls _rabbit-hole.png.extracted/
109F5.zip  129  129.zlib  data.txt
```
The `data.txt` was from inside the `zip` file. The `zlib` file and it's decompressed `129` file are just the image data, so we can ignore them.

The `data.txt` file contained:
```
> cat data.txt 
0000000: 1f8b 0808 dba0 f556 0003 6461 7461 3200  .......V..data2.
0000010: 0bf0 6666 1161 6060 e060 305a 52e9 f1e2  ..ff.a``.`0ZR...
0000020: 7ef0 8f5f 409e 2423 4844 8621 2d31 275d  ~.._@.$#HD.!-1']
0000030: afa4 a224 3484 9381 79c1 82af 61a7 81b8  ...$4...y...a...
0000040: b482 9b81 91e5 0533 0303 9898 fcfe 5942  .......3......YB
0000050: c486 07d9 12cc 9bb7 2fef 5b7e d2f5 fd56  ......../.[~...V
0000060: b995 dc29 bc12 337a 5a5b 598b 8c3f 6d92  ...)..3zZ[Y..?m.
0000070: aa99 252d 7e6e 4b4e 9c45 e1b3 7353 a5c5  ..%-~nKN.E..sS..
0000080: dfcd 3929 51f0 fc50 a7b0 f833 0bb9 9c8c  ..9)Q..P...3....
0000090: d3d9 59fb be64 5c12 540d bbb6 cddb 5bef  ..Y..d\.T.....[.
00000a0: ed4c 9194 cf5d 8f59 ea1f 304f 5cd9 3953  .L...].Y..0O\.9S
00000b0: 5f25 aceb 64c4 bab0 7f9a 87ce c8fc fdbe  _%..d...........
00000c0: cb44 9e5f fcfd 7ed7 3d36 bbab 2a6f ed89  .D._..~.=6..*o..
00000d0: 544d d151 515d a2b3 e4db 9909 3314 1595  TM.QQ]......3...
00000e0: 0e1e 59fe b7fa 9a4c b7e6 649e 5de9 f36a  ..Y....L..d.]..j
00000f0: 17ed adb4 7dfe d354 49bb daec 7ff1 9c98  ....}..TI.......
0000100: 5fef b866 dffc 5d7c f22b 6bfc f498 f766  _..f..]|.+k....f
0000110: f78d efb6 ecf6 775f 7d27 efdf d4dc 6fed  ......w_}'....o.
0000120: 62d3 fda2 36fc 38fe a868 e765 ebe8 a819  b...6.8..h.e....
0000130: da5b 536e e6dd 8d9b a370 e9b2 48c6 81a7  .[Sn.....p..H...
0000140: 6501 ce4d 534d ce09 bbb5 2d75 bfbc 5ac2  e..MSM....-u..Z.
0000150: 4442 8221 c09b 9149 8e19 57e8 4930 c0c0  DB.!...I..W.I0..
0000160: 9246 1089 084b 5648 58a2 8563 8037 2b1b  .F...KVHX..c.7+.
0000170: 481d 2310 fa01 691b 4690 0100 8ec1 4e63  H.#...i.F.....Nc
0000180: a001 0000
```
You can see that this is a `hexdump` of something, and it includes the name `data2` near the top. Lets turn this `hexdump` back into a binary file with `xxd`:
```
> cat data.txt | xxd -r > output
```
Now we need to know what type of file it is so we can deal with it:
```
> file output 
output: gzip compressed data, was "data2", last modified: Fri Mar 25 20:34:35 2016, from Unix
```
So I renamed the file `data2.gz` and ran `gunzip` on it:
```
> gunzip data2.gz
```
This created the file `data2`, so again I needed to check the type of that file:
```
> file data2 
data2: Zip archive data, at least v2.0 to extract
```
This is a `zip` file, so I renamed it `data2.zip` and ran `unzip` on it:
```
> unzip data2.zip
```
That gave us a file named `falg.txt`. We must be getting close, right! I ran `cat` on that file, but it wasn't text, so again I ran `file` to find out it's type:
```
> file falg.txt
falg.txt: gzip compressed data, last modified: Fri Mar 25 20:33:36 2016, from Unix
```
So I renamed `falg.txt` to `falg.gz` and ran `gunzip`:
```
> gunzip falg.gz
```
Again, when I ran `cat` on the file it still wasn't text, so I ran `file` on it again:
```
> file falg
falg: POSIX tar archive (GNU)
```
I renamed it `falg.tar` and ran `tar` over it:
```
> tar -xvf falg.tar
data4.exe
```
This gave me `data4.exe`. Again I checked the type:
```
> file data4.exe 
data4.exe: bzip2 compressed data, block size = 900k
```
I renamed the file, this time to `data4.bz2`, and ran `bzip2` over it:
```
> bzip2 -d data4.bz2
```
This gave me the file `data4` which I checked the type of yet again:
```
> file data4
data4: gzip compressed data, was "flag.bin", last modified: Fri Mar 25 20:28:48 2016, from Unix
```
OK. At least now I can see the name `flag.bin`, so, hopefully, were really nearing the end. Lets rename and `gunzip` this:
```
> gunzip data4.gz
```
This gives us a file named `data4`. Lets check the type:
```
> file data4 
data4: ASCII text
```
This final file contained the flag!
