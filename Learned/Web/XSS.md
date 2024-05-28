# XSS
### XSS Senza parentesi
Puoi sostituire le parentesi con i backtick, inoltre puoi fare XSS con hex e HTML entities.

# DOMPurify
### HTMX
Puoi bypassare DOMPurify con HTMX (se usato) [HTMX-SS](https://blog.criticalthinkingpodcast.io/p/0-days-htmx-ss-with-mathias)
La stessa cosa può avvenire con le direttive Angular e in generale con librerie/framework che lo permettono

### XML BUG (patchato)

Bypasso di DOMPurify con le processing instruction XML [XML DOMPurify Bypass](https://blog.slonser.info/posts/dompurify-node-type-confusion/)

### CID: Protocol

DOMPurify allows the cid: protocol, which does not URL-encode double quotes. This can be exploited to inject malicious payloads:
```
<a id="yourID" name="yourName" href='cid:"onerror=alert(1)//'></a>
```
In this example, an encoded double quote is injected and decoded at runtime, potentially leading to script execution when processed by the browser.

