# Level01 – SnowCrash

## Objective
Recover the password for `flag01` by analyzing system files and cracking a DES hash.

## Reconnaissance

```bash
cat /etc/passwd
...
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```

The `/etc/passwd` file contained a 13-character DES hash for `flag01`. Unlike modern systems that store hashes in `/etc/shadow`, this legacy system exposes the hash directly in `/etc/passwd`.

## Exploitation
The string `42hDRfypTqqnw` is a 13-character hash, whick indicates a legacy Unix DES-based hash.
Since the format matches DES, we can se John The Ripper to crack it.

![John the Ripper cracking the DES hash](images/crack-passwd.png)

## Flag

![Level01 Flag Screenshot](images/level01_flag.png)
