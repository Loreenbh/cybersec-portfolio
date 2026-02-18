# Level03 – SnowCrash

## Objective
Exploit a vulnerable SUID binary to execute code with `flag03` privileges.

## Reconnaissance
The binary level03 is SUID and owned by `flag03`.
Static and dynamic analysis (`strings`, `ltrace`) reveal that the binary calls system(`/usr/bin/env echo Exploit me`), relying on the environment’s PATH to locate `echo`.
```bash
system("/usr/bin/env echo Exploit me");
```
The program executes `echo` using `/usr/bin/env`, meaning it relies on the `PATH` environment variable to locate the binary.
Because the program does not use an absolute path (e.g. `/bin/echo`), it is vulnerable to PATH hijacking.

If an attacker controls`PATH`, they can execute a malicious binary named `echo` before the legitimate one.

## Exploitation

- Create a malicious `echo` script in a writable directory:
```bash
#!/bin/sh
/bin/getflag
```
- Prepend the directory to PATH.
- Execute the SUID binary.

The program runs the malicious `echo` with `flag03` privileges.

## Flag
![Level03 Flag Screenshot](images/level03_flag.png)
