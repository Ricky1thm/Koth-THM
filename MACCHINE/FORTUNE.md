# FORTUNE

## ENUMERAZIONE
 
scan con nmap per verificare le porte aperte e le versioni dei servizi che ci girano sopra
```bash
nmap fortune.thm -sV -sC 
```

a questo punto abbiamo trovato la porta 3333 molto interessate...(se non riuscite a vederla rifare lo scan solo con l'opzione -p-)
se notate facendo partire successivamente un altro scan sulla porta 3333 e dandogli l'opzione la porta ci risponde con una stringa impresentabile con un = in fondo
bene ragazzi quello è un base64 cosa fare quando abbiamo un base64? decodarlo ovvio! cosi facendo ci restituisce infomazioni interessanti. quindi ora sappiamo che 
dentro quella porta possiamo aspettarci un file zip. andiamo a prenderlo.

## NETCAT

una volta trovata qusta porta perchè non ci mettiamo in ascolto e vediamo cosa ci dice?

```bash
nc fortune.thm 3333 | base64 -d
```

a questo punto sono in ascolto della porta e gli sto dicendo di decodarmi tutto quello che invia.
ed ecco qua quello che ci aspettavamo un file creds.txt.

con questo comando successivamente lo prendiamo.

```bash
nc fortune.thm 3333 | base64 -d > test.zip
```
## JOHN

facendo l'unzip del file a quanto pare serve una password.

```bash
zip2john test.zip > hash.txt
```

andiamoci a prendere l'hash della password e crackiamola!
```bash
john hash.txt
```
e ecco la password! unzippiamo il file 
```bash
unzip test.zip
```

```bash
cat creds.txt
```

BINGO! abbiamo l'utente è la password

entriamo in ssh

```bash
ssh fortuna@fortune.thm
password:
```

## PRIVILEGE ESCALATION

prima che inizi consiglio di visitare il sito GTFO.BINS per spunti interessanti sul privilege escalation

vediamo che comandi da superuser possiamo lanciare

```bash
sudo -l
```

per il resto lascio a voi il ragionamento 

CONSIGLIO!!!(dovete aprire una shell)



