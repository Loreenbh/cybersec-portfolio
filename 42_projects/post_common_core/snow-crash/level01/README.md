# Level01 – SnowCrash

## Objective
Recover the password for `flag01` by analyzing system files and cracking a DES hash.
---

## Reconnaissance

```bash
cat /etc/passwd
```

### Output
```
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash

```
The second field contains a 13-character encrypted string:
`42hDRfypTqqnw`
Unlike modern systems that store hashes in `/etc/shadwow`, this one exposes the password 
hash directly in `/etc/passwd`.

##Exploitation
The string `42hDRfypTqqnw` is a 13-character hash, whick indicates a legacy Unix DES-based hash.
DES hashes:
- Are 13 characters long
- Were historically stored directly inside `/etc/passwd`
Since the format matches DES, we can se John The Ripper to crack it.
![John the Ripper cracking the DES hash](images/level01_crack-passwd.png)

##Flag
![Level01 Flag Screenshot](images/level01_flag.png)
