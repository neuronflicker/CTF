# xorly
**Category:** Crypto

**Points:** 10

**Description:**

Author: crowell

> **Files:** xorly.py

## Write-up
> **Note:** This write-up uses Python2. Scripts and commands in Python3 may differ
The Python code provided with this was:
```python
#!/usr/bin/env python2

def encrypt(plaintext, key):

    ciphertext = []
    for i in xrange(0, len(plaintext)):
        ciphertext.append(ord(plaintext[i]) ^ ord(key[i%len(key)])) 

    return ''.join(map(chr, ciphertext))

decrypt = encrypt

'''
I'll give you a sample of how this works:

Plaintext: 
"Here is a sample. Pay close attention!"

Ciphertext: (encoded in hex)
2e0c010d46000048074900090b191f0d484923091f491004091a1648071d070d081d1a070848

Flag: (encoded in hex, encrypted with the same key)
0005120f1d111c1a3900003712011637080c0437070c0015
'''
```

Looking at the code (and based on the name) this encryption uses XOR to encode a string with a key. This means if we have the original text and the cipher, we should be able to XOR these to get the original key.

I made an assumption that the items in the comment were the plaintext used, the cypher resulting from it, and the flag. If we can find the key, we can decode the flag. This uses the principle that if A XOR B = C, then A XOR C = B and B XOR C = A.

First I checked that the text in the comment was related to the Ciphertext in the comment:
```python
>>> len('Here is a sample. Pay close attention!')
38
>>> len('2e0c010d46000048074900090b191f0d484923091f491004091a1648071d070d081d1a070848')
76
```
As we can assume 2 hex characters per letter, it seems that the Ciphertext is related to the plaintext.

I added two functions to the code. `toHex()` and `toChar()`:
```python
def toHex(str_val):
  hex_val = []
  for c in str_val:
    # Convert to hex and remove the hex prefix
    nxt_hex = hex(ord(c))
    nxt_hex = nxt_hex.replace('0x', '')

    # If it's only one character, put a 0 at the beginning
    if len(nxt_hex) == 1:
      nxt_hex = '0' + nxt_hex

    hex_val.append(nxt_hex)

  return ''.join(hex_val)

def toChar(hex_val):
  str_val = []
  for h in range(0, len(hex_val),2):
    nxt_hex = hex_val[i:i+2]
    str_val.append(chr(int(nxt_hex,16)))

  return ''.join(str_val)
```

These will allow me to test various things out. First I tested these functions with:
```python
result = toHex("Help")
print('result is: %s' % result)
result = toChar(result)
print('result is: %s' % result)
```

The output of running this was:
```bash
> python xorly.py
result is: 48656c70
result is: Help
```
So the functions work as expected. Now I try some experiments with known data:
```python
plaintext = 'Hello, World!'
key = 'my key'
result = encrypt(plaintext, key)
result = toHex(result)
print('Result as hex: %s' % result)
print('Length should be %d, is %d' % ((len(plaintext) * 2), len(result)))
result = toChar(result)
res_key = decrypt(plaintext, result)
print('Res Key is: %s' % res_key)
flag = "cheese"
flag_enc = encrypt(flag, key)
flag_enc = toHex(flag_enc)
print('Flag enc as hex: %s' % flag_enc)
print('Length should be %d, is %d' % ((len(flag) * 2), len(flag_enc)))
flag_enc = toChar(flag_enc)
flag_dec = decrypt(flag_enc, res_key)
```

The output of running this was:
```bash
> python xorly.py
Result as hex: 251c4c070a554d2e4f19091d4c
Length should be 26, is 26
Res Key is: my keymy keym
Flag enc as hex: 0e11450e161c
Length should be 12, is 12
Flag dec is: cheese
```
When run, we can see this returns the key (repeated as it uses modulus), and decrypts the flag with the recovered key. As our flag is shorter than the Ciphertext in the real data, we can just use this repeated key to decrypt it. Otherwise we'd have to split up the key in to one instance. 

Now I can run it on the actual data:
```python

plaintext = 'Here is a sample. Pay close attention!'
ciphertext = '2e0c010d46000048074900090b191f0d484923091f491004091a1648071d070d081d1a070848'
flag_enc = '0005120f1d111c1a3900003712011637080c0437070c0015'

ciphertext = toChar(ciphertext)
dec_key = decrypt(plaintext, ciphertext)
print('Dec Key is: %s' % dec_key)
flag_enc = toChar(flag_enc)
flag_dec = decrypt(flag_enc, dec_key)
print('Flag dec is: %s' % flag_dec)
```

This worked, and gave me the flag.
