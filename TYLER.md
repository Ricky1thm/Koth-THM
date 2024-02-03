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
