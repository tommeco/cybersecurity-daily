# GIORNO 3 - HASH CRACKER CON DIZIONARIO (MD5/SHA)

## Descrizione
Implementazione di un hash cracker in Python che supporta MD5, SHA1, SHA256 e SHA512. 

Questi script includono: 
- dictionary attack base 
- rilevamento automatico dell'algoritmo 
- effetto valanga
- attacco con regole hashcat-style per generare varianti automatiche delle parole. 

---

## Obiettivo didattico
Capire come funzionano le funzioni hash, perchè vengono usate per conservare le password nel database e come un attaccante può tentare di risalire alla password originale partendo dall'hash. 

---

## Concetti trattati 
Una funzione hash trasforma qualsiasi input in una stringa di lunghezza fissa. Ha tre proprietà fondamentali: è deterministica (stesso input produce sempre lo stesso output), è unidirezionale (dall'hash non puoi risalire all'input originale) e presenta l'effetto Valanga (cambiare un carattere cambia completamente l'hash). 

Gli algoritmi supportati nel progetto sono: 
- MD5: Che ha una lunghezza di 32 caratteri e un esempio di output potrebbe essere questo (5d41402abc4b2a76b9719d911017c592)

- SHA1: Che ha una lunghezza di 40 caratteri e un esempio di output è: (aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d)

- SHA256: Che ha una lunghezza di 64 caratteri e un esempio è: 
(cf24dba5fb0a30e26e83b2ac5b9e29e...)

- SHA512: Che ha 128 caratteri. 

L'effetto valanga è stato dimostrato nel codice con la parola "ciao": anche aggiungere uno spazio finale o cambiare una sola lettera in maiuscolo produce un hash completamente irriconoscibile rispetto all'originale. 

Il dictionary attack è la strategia di attacco principale: per ogni parola nella wordlist viene calcolato l'hash e confrontato con il target. Se corrispondono, la password è trovata. Per aumentare l'efficacia dell'attacco è stato implementato un sistema di regole hashcat-style che genera varianti automatiche per ogni parola. 

Questo moltiplica ogni parola in circa 44 varianti, portando una wordlist di 34 parole base a 1497 candidati totali.

--- 

## Struttura del codice 
giorno3_hash_cracker.ipynb
│
├── Cella 1 — Imports e setup
│   └── hashlib, os, lista algoritmi supportati
│
├── Cella 2 — Funzioni hash base
│   ├── calcola_hash(testo, algoritmo)
│   └── identifica_algoritmo(hash_string)
│
├── Cella 3 — Effetto valanga
│   └── mostra_effetto_valanga(testo)
│
├── Cella 4 — Dictionary attack base
│   └── dictionary_attack(hash_target, wordlist, algoritmo)
│
├── Cella 5 — Wordlist da file
│   ├── carica_wordlist_da_file(percorso)
│   └── crea_wordlist_italiana()
│
├── Cella 6 — Attacco con regole
│   ├── genera_varianti_v2(parola)
│   └── attacco_con_regole(hash_target, wordlist, algoritmo)
│
└── Cella 7 — Sfida finale
    └── 3 hash da craccare con attacco_finale()


--- 
## Funzioni implementate 
- calcola_hash(testo, algoritmo) : Calcola l'hash di una stringa con l'algoritmo scelto usando hashlib.new(), che supporta tutti gli algoritmi standard. 

calcola_hash("hello", "md5")
 → "5d41402abc4b2a76b9719d911017c592"

calcola_hash("hello", "sha256")
→ "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824"


- identifica_algoritmo(hash_string): Identifica l'algoritmo dell'hash analizzando la lunghezza della stringa esadecimale: 32 caratteri per MD5, 40 per SHA1, 64 per SHA256, 128 per SHA521. 

identifica_algoritmo("5d41402abc4b2a76b9719d911017c592")
 → "MD5"  (32 caratteri)

- genera_varianti_v2(parola): Genera tutte le varianti di una parola applicando le regole hashcat-style. Ogni parola produce circa 44 varianti. 

genera_varianti_v2("napoli")
 → ["napoli", "NAPOLI", "Napoli", "n4p0l1",
   "napoli123", "Napoli2024", "Napoli1926!", ...]


- attacco_finale(hash_target, wordlist_base, algoritmo): Funzione principale che espande la wordlist con tutte le varianti e cerca la corrispondenza hash per hash. 

--- 
## Risultati della sfida finale 
Tutti e 3 gli hash sono stati craccati con l'attacco con regole partendo da una wordlist di 34 parole base espansa a 1497 varianti. 

## Errori incontrati e soluzioni: 
### Errori 1 - indentationError nella cella 5 

- Causa: indentazione errata alla riga 42 troppi spazi prima di un'istruzione dentro un blocco.

- Soluzione: identifica visivamente l'istruzione con identazione errata e corretta manualmente. Confermata la soluzione consultando Gemini. 

- Lezione appresa: Python è sensibile all'indentazione un blocco if, for o def deve avere indentazione consistente in tutto il suo corpo. 


### Errore 2 - Hash 1 non trovato (b14a7b880..) 

- Causa: l'hash hardcoded nel codice originale non corrispondeva a nessuna parola della wordlist. 

#### debug eseguito 
print(calcola_hash("secret", "md5"))
# → 5ebe2294ecd0e0f08eab7690d2a6ee69  ≠  b14a7b80...
```

**Soluzione:** rigenerare gli hash direttamente nell'ambiente Colab invece di usare hash hardcoded, così da garantire che mittente e destinatario usino lo stesso encoding.

**Lezione:** usare sempre `utf-8` esplicitamente e generare i dati di test nello stesso ambiente in cui si esegue il codice.

---

### Errore 3 — `NameError: attacco_finale is not defined`

**Causa:** la funzione era stata definita in una cella precedente ma il runtime era stato riavviato — Colab aveva perso tutte le definizioni in memoria.

**Soluzione:** raccogliere tutto il codice necessario in una singola cella autonoma che non dipende da nessuna cella precedente.

**Lezione:** in Colab dopo ogni riavvio bisogna rieseguire tutte le celle — le celle critiche devono essere il più possibile autonome.

---

## Perché MD5 non è sicuro per le password

MD5 è stato progettato per essere veloce e può calcolare circa 60 miliardi di hash al secondo su una GPU moderna, rendendolo inutilizzabile per proteggere le password. SHA1 e SHA256 non sono molto meglio senza salting.

| Algoritmo | Hash/secondo (GPU moderna) | Sicuro per password? |
|---|---|---|
| MD5 | ~60 miliardi | No |
| SHA1 | ~20 miliardi | No |
| SHA256 | ~10 miliardi | No (senza salting) |
| bcrypt | ~20.000 | Sì |
| Argon2 | ~1.000 | Sì |

Gli algoritmi progettati appositamente per le password come bcrypt e Argon2 sono deliberatamente lenti, rendendo il brute force computazionalmente proibitivo anche con hardware potente.

La difesa principale contro il dictionary attack è il **salting**: aggiungere una stringa casuale alla password prima di calcolare l'hash rende inutili le wordlist precalcolate (rainbow tables).
```
Senza salt:  hash("password") → sempre uguale per tutti gli utenti
Con salt:    hash("password" + "xK9#mL2p") → unico per ogni utente

## MORALE DI QUESTO PROGETTO: 
Il concetto più importante appreso è che MD5 e SHA non sono algoritmi sicuri per le password in produzione la velocità che li rende utili per i checksum li rende pericolosi per l'autenticazione.