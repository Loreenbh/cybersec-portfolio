# Level01 – SnowCrash

## Objective
Recover the password for `flag01` by analyzing system files and cracking a DES hash.

## Reconnaissance
The `/etc/passwd` file contains the following entry for `flag01`:
```bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
```
This 13-character string is a legacy Unix DES hash, exposed directly in `/etc/passwd` (modern systems store hashes in `/etc/shadow`).

## Exploitation
The string `42hDRfypTqqnw` is a 13-character hash, whick indicates a legacy Unix DES-based hash.
Since the format matches DES, we can use John The Ripper to crack it.
![John the Ripper cracking the DES hash](images/crack-passwd.png)

## Flag
![Level01 Flag Screenshot](images/level01_flag.png)
