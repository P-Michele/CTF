## Sign/encrypt
Molte librerie sono vulnerabili a: sign/encrypt confusion attack https://i.blackhat.com/BH-US-23/Presentations/US-23-Tervoort-Three-New-Attacks-Against-JSON-Web-Tokens-whitepaper.pdf

## KID
Il parametro kid specifica che file viene usato per firmare un JWT. Un programmatore non deve fidarsi del parametro kid proveniente dal JWT (possibile payload /dev/null)
