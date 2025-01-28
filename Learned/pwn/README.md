# NX
NX (se abilitato) non è irreversibile. Può essere definita una zona eseguibile in memoria tramite mprotect

# Seccomp
Se seccomp disabilita exceve puoi utilizzare open+read+write per aprire il file della flag + scrivere la flag in memoria + leggerla in stdout


# PwnDbg

Ricerca una striga (di solito la shell) per ottenerne l'indirizzo:

```
 search -t string "/bin/sh" 
```

# Leak
I leak, ottenuti in qualsiasi modo, vanno calcolati nel seguente modo:

```
base = leak() - leak_off
gadget1 = base + gadget1_off
gadget2 = base + gadget2_off
ecc....
```
# Linux
Puoi vedere le librerie caricate dinamicamente (quindi anche la libc) dtramite il comando:
```
ldd <file_name>
```
Per vedere informazioni sul file (stripped, not stripped, architettura) puoi usare:
```
file <nome_file>
```

# Lista delle syscall 

[64 bit](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)

[32 bit](https://x86.syscall.sh/)

[Tutte le syscall](https://syscalls.w3challs.com/)

# Sito che indica la versione della libc dato un leak

[Libc Database](https://libc.rip/)

# Passaggio degli argomenti a 64 bit:
RDI, RSI, RDX, RCX, R8, R9, ..., RSP + 0x10 + (i-6)*8

# Heap
## Comandi utili:

```bins``` mostra lo stato dei bin

```heap ``` mostra lo stato dell'heap, con chunk e metadati

```vis``` mostra uno stato colorato dell'heap in hexdump

```try_free``` guarda se ```free(addr)``` avrebbe successo

```malloc_chunk``` guarda i metadati di un chunk ad un indirizzo

```find_fake_fast``` aiuta a forgiare falsi fastbin chunk overlappati all'indirizzo specificato.

