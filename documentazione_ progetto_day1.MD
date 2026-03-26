# DAY 01: Ceaser Cipher Deep Dive
## 📖 Panoramica: 
Il cifrario di Cesare è uno dei più antichi e semplici metodi di cifratura per sostituzione (con cifratura per sostituzione si intende un metodo di cifratura in cui ogni lettera di un messaggio da cifrare viene sostituita con un'altra lettera o simbolo, seguendo una regola chiamata chiave).

In questo progetto ho creato degli script in python per capire e approfondire meglio il funzionamento della crittografia simmetrica e per vedere quali problemi possono riscontrare gli algoritmi con chiavi troppo piccole. 


## Concetti chiave esplorati
- *Crittografia Simmetrica*: Ho utilizzato la stessa chiave (k) per le operazioni di cifratura e decifratura 

- *Aritmetica modulare*: L'aritmetica modulare è quel metodo che permette di far girare e quindi far ritornare al punto di partenza vari parametri. Nel caso dell'alfabeto, ci sono 26 lettere: quando arrivi alla Z e vai avanti non fai che ripetere il punto iniziale. Nel caso degli script ho scritto % 26. % 26 è l'operatore che attua questo metodo ovvero prende un numero e lo riporta sempre dentro l'intervallo delle 26 lettere, evitando di uscire fuori dall'alfabeto. 

- *Principio DRY (Don't Repeat Yourself)*: Il principio di DRY è il principio che fa si che un determinato tipo di codice non venga scritto più volte. Nel mio caso, con cifra() sposto le lettere in avanti e invece con decifra() usa la stessa tecnica ma le sposto indietro, cosi facendo evito di avere codici duplicati, il programma è più snello e è più da modificare. 

- *Analisi di sicurezza*: Nel programma volevo dimostrare che se si hanno pochi chiavi un attaccante può trovarle velocemente provandole tutte. Quindi tramite li script ho dimostrato che è molto facile rompere il sistema. 


---

# ARCHITETTURA DEL CODICE:
### CIFRA(testo, shift)
- Logica: En​(x)=(x+n)mod26
- Scopo: La cifratura in cui ogni lettera viene spostata di un certo numero di posizioni. 

### decifra(testo, shift)
- logica: Dn​(x)=(x−n)mod26
- Scopo: Invertimento del processo utilizzando la stessa logica di cifratura ma al contrario. 

### brute_force(testo)
- logica: Loop i=1…25
- Scopo: Esporre le vulnerabilità del cifrario di cesare mettendo in stampa tutte le permutazioni possibili. 


# DEMO RAPIDA 
### Cifratura con shift ROT13
secret = cifra("Cybersecurity è TOP!", 13)
print(f"Cifrato: {secret}") 
### Output: "Plorefrphevgl è GBC!"

### Decifratura immediata
original = decifra(secret, 13)
print(f"Originale: {original}")
### Output: "Cybersecurity è TOP!"


--- 

# ANALISI TECNICA: COSA HO IMPARATO 
- **GESTIONE DEI CARATTERI NON ALFABETICI**: Ho implementato un filtro per ignorare simboli, numeri e spazi, garantendo che l'integrità del messaggio rimanga intatta senza corrompere i caratteri speciali. 

- **IL RUOLO DI ASCII**: Ho approfondito l'uso di ord() e chr() per mappare i caratteri in valori numerici, utilizzando una base dinamica (start=65) per le maiuscole, 97 per le minuscole. 

- **VULNERABILITA' CRITICA**: Nonostante la bellezza matematica, il cifrario di Cesare è vulnerabile non solo al brute force, ma anche all'analisi delle frequenze. 
