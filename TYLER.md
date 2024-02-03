# Tyler

# enum
partiamo con una enumerazione e scan dei serizi


```bash
nmap tyler.thm -sV -sC 
```
ed ecco spuntare a schermo la porta 445

coleghiamoci e vediamo cosa ci offre

# SMB

```bash
smbclient -L \\\\tyler.thm\\
```

guardiamo se colleghandoci in anonimo in tutte e tre le directory abbiamo qualche hint
```bash
$ root@kali:# smbclient \\\\tyler.thm\\public
```



