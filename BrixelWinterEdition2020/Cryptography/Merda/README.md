# Merda
**Category:** [Cryptography](../README.md)

**Points:** 5

**Description:**

An Italian messenger was caught during the war

He was carrying a piece of paper that read: ymj kqfl nx gwncjqHYK{uneefsfutqn}

Upon torturing the messenger for an explaination, he gestured a V with his fingers. The english guard took it as an insult and killed him right on the spot.

Maybe he just wanted some peace?

## Write-up
We know that the flags for the [Brixel CTF](https://ctf.brixel.space) are in the format `brixelCTF{<flag>}`, so we can guess that the `gwncjqHYK{uneefsfutqn}` part of the message is the flag. If we assume a Caeser cipher (as the descriptions talks about 'Italian'), we can see that *g* becomes *b* if we subtract 5 letters, and *w* becomes *r*. As the messenger gave a 'V' sign (which is 5 in Roman numerals), we can see that this is probably our solution.

So we now use a Caesar cipher of 5 characters backwards over the whole message we get:
  *the flag is brixelCTF{&lt;flag was here&gt;}*

> Note: I did this conversion manually, but there are online converters such as [Cryptii](https://cryptii.com/pipes/caesar-cipher).

