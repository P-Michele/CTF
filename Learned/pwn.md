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


