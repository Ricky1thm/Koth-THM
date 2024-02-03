# OFFLINE



## ENUM
avviamo una scansione sulla macchina
```bash
sudo nmap -sC -sV OFFLINE.thm
```

successivamente avviamo uno scan con i vari script di nmap per quanto riguarda smb
```bash
sudo nmap -sV -p 445 --script *smb* OFFLINE.thm
```

lo script ci restituisce un nome utente SCARRA e per quanto riguarda il server ci dice che l'smb gira con la versione SMBv1
molto utile perchè ci specifica una versione con vulnerabilità critiche andiamo ad analizzare meglio la vulnerabilità

## MS17-010

consente di inviare sul protocollo richieste smb che permettono successivamente all'utente di assumere privilegi di 
/NT-autority system e successivamente inviare codice come admin di sistema
usiamo questa vulnerabilità per entrare sul sistema 

avviamo metasploit
```bash
msfconsole
```
entriamo sull'exploit
```bash
use exploit/windows/smb/ms17_010_psexec
```
impostiamo tutte le infomazioni richieste dall'exploit e lanciamolo


BINGO! siamo dentro con l'utente NT-autority

