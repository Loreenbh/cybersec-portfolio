# Level03 – SnowCrash

## Objective
Exploit a vulnerable SUID binary to execute code with `flag03` privileges.

## Reconnaissance
The binary level03 is SUID and owned by `flag03`.
Static and dynamic analysis (`strings`, `ltrace`) shows that it calls:
```bash
system("/usr/bin/env echo Exploit me");
```
- The program uses `/usr/bin/env` to locate `echo`, relying on the PATH environment variable.
- Because it does not use an absolute path (e.g. `/bin/echo`), the binary is vulnerable to PATH hijacking.
- An attacker controlling `PATH` can execute a malicious binary named `echo` before the legitimate one.

## Exploitation
- Create a malicious `echo` script in a writable directory:
```bash
#!/bin/sh
/bin/getflag
```
- Prepend the directory to `PATH` environment variable.
- Execute the SUID binary.
The program runs the malicious `echo` with `flag03` privileges.

## Flag
![Level03 Flag Screenshot](images/level03_flag.png)
