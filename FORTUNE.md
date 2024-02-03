# FORTUNE

come sappiamo si parte sempre con uno scan più o meno completo della rete per capire cosa dobbiamo affrontare 

### enum

```bash
nmap fortune.thm -sV -sC 
```

a questo punto abbiamo trovato la porta 3333 molto interessate...(se non riuscite a vederla rifare lo scan solo con l'opzione -p-)
se notate facendo partire successivamente un altro scan sulla porta 3333 e dandogli l'opzione la porta ci risponde con una stringa impresentabile con un = in fondo
bene ragazzi quello è un base64 cosa fare quando abbiamo un base64? decodarlo ovvio! cosi facendo ci restituisci infomazioni su cosa quella porta lavora. quindi ora sappiamo che 
dentro quella porta possiamo aspettarci un file zip. andiamo a prenderlo.

### netcat

una volta trovata qusta porta perchè non ci mettiamo in ascolto e vediamo cosa ci dice 

```bash
nc fortune.thm 3333 | base64 -d
```

a questo punto sono in ascolto della porta e gli sto dicendo di decodarmi tutto quello che invia.
ed ecco qua quello che ci aspettavamo un file creds.txt

con questo comando successiamo lo andiamo a prenderlo 

```bash
nc fortune.thm 3333 | base64 -d > test.zip
```

