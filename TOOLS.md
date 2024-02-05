# TOOLS E TECNICHE UTILIZZATE

## Nmap

utilizzato per scansionare 1 o più indirizzi nelle varie macchine verrà utilizzato SEMPRE quindi mi sembra giusto aggiungere 2 parole su questo tool.
qua sotto vi lascio dei comandi con una piccola descrizione su cosa vanno a fare.

questo comando va ad analizzare le porte e su queste va ad eseguire script che nmap contiene nel suo database. usato principalmente per ottenere più informazioni sul target
```bash
nmap indirizzo IP -sC 
```
analizza il target e quando trova delle porte aperte va a controllare il servizio e la versione che ci gira sopra. utilizzato per scoprire se girano servizi vulnerabili sulle porte
```bash
nmap indirizzo IP -sV 
```

va ad eseguire le stesse cose delle opzioni aggiuntive e in più aggiunge uno scan del sistema operativo per capire su che tipo di macchina ci troviamo
```bash
nmap indirizzo IP -A 
```
esegue la verifica del sistema operativo che si trova sul target.
```bash
nmap indirizzo IP -O 
```

questi sono i comandi principali di nmap che vi ho elencato ma se siete curiosi vi consiglio di andare a guardare la documentazione ufficiale https://nmap.org.

## Hydra 

tool utilizzato per fare brute force su servizzi(ssh, ftp, sql, smb....) ottimo nel bruteforce proprio perchè può essere impostato a threat cioè permetti di impostare quanto rumoroso può essere 
quindi molto utile in situazioni dove fare poco rumore è fondamentale. qua sotto vi elenco alcune possibilità di questo comando

questo comando va ad eseguire fino ad un massimo di 5 sessione dove prova tutte le password presenti nella wordlist sul servizio ssh con l'utente ricky
```bash
hydra -l ricky -P /usr/share/wordlist/rockyou.txt indirizzo IP ssh -t 5
```

questo comando va ad eseguire fino ad un massiomo di 5 sessioni dove prova tutti i nomi presenti nella userlist.txt e le password persenti nel file rockyou.txt sul servizio ftp

```bash
hydra -L /home/download/userlist.txt -P /usr/share/wordlist/rockyou.txt indirizzo IP ftp -t 5
```
anche qua se siete curiosi vi lascio il sito di kali dove vengono ampliati i miei esempi https://www.kali.org/tools/hydra/.


## SSH

un servizio molto utilizzato per la connesione in remoto tra infrastrutture vi lascio dei comandi principali per poter iniziare a prenderci la mano 

questo comando permette di collegarci in ssh con l'utente ricky e l'indirizzo ip della macchina remota
```bash
ssh ricky@indirizzo IP
```

questo comando ci fa connettere con le stesse variabili di prima, l'unica differenza e che tramite un id_rsa non avremmo bisogno di inserire la password 
```bash
ssh ricky@indirizzo IP -i id_rsa
```

## FTP

servizio utilizato per il trasferimento file, solitamente si trova sulla porta 21 e ci permette di conneterci in remoto per scaricare o inviare file

```bash
ftp ricky@indirizzo IP
```

## NetCat

viene utilizzato per mettersi in ascolto su una determinata porta ed ascolta quello che viene inviato su questa porta 

con questo comando sto aprendo un socket sulla porta 4444 e mi sto mettendo in ascolto di quello che arriva su questa
```bash
nc -lvnp 4444
```
con questo comando mi metto in ascolto su una aporta da uno specifico target, in questo la porta è 8080
```bash
nc indirizzo IP 8080
```
## John
tool di kali utilizato per decryptare hash molto utile quando si hanno delle password sotto forma di hash perchè ci permette di fare un confronto tramite la modalità di encrypting l'hash della password 
con delle hash prensenti in una wordlist di password

```bash
john -w /usr/share/wordlist/rockyou.txt hash.txt
```
