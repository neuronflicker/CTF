# A song...
**Category:** [Programming](../README.md)

**Points:** 10

**Description:**

I wrote this song

it seems I'm pretty bad at it, but hey! it could get you a flag :)

**Files:** a_song.txt

## Write-up
The song text contained:
```
(intro)
Shout "brixelCTF{" !!!

Brixel is a hackerspace
It's not like any other place

Your skill is hopefully the best
This CTF is the test
put your skill into the test
(-and-) let your score be "blessed"

(chorus)
The challenges are serious
Your skill is mysterious
Build your skill up, up, up (-up,up-)
Knock the challenges down
your skill is true,
your skill is right!
Knock the challenges down
your score is taking flight!

(verse1)
put This CTF into your skill
put Brixel into your Heart (-or not, hey! just chill!-)

the hype is getting to the top,
the beat is ready to drop,
build the hype up!
build the hype up!
build the hype up!

whisper the challenges,
say your score,
Shout the hype, (-and-)
SCREAM YOUR SKILL! (-m-M-M-MONSTERKILL!!-)

(chorus)
The challenges are serious
Your skill is mysterious
Build your skill up, up, up (up,up)
Knock the challenges down
your skill is true,
your skill is right!
Knock the challenges down
your score is taking flight!

(verse2)
Happy Holidays is a wish,
Brixel is wishing you today
Santa is now leaving
(-riding on his sleigh-)

This was fun
This was grand
Turn up your score
Turn up your skill

put your heart into your skill
put your skill into the test
say your score (-because you ARE the best-)

Say Happy Holidays
Say Brixel and "}"

(fin)
```
When looking at the text we noticed that `brixelCTF{`, `blessed` and `}` were the only bits in quotes, so we tried that as the flag. It didn't work.

Staring at the text for a while, we noticed that it looked a little like pseudocode. Phrases like `say` or `shout`, and `put` `X is Y` appear throughout.

We searched Google for anything related to song lyrics as a programming language and found [Rockstar](https://github.com/RockstarLang/rockstar), which creates programs that are song lyrics.

By searching a little further, we found [this site](https://codewithrockstar.com/online) where we could copy the song text and get our flag.
