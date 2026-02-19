# Level13 – SnowCrash

## Objective
Bypass a UID check in a SUID binary to retrieve the flag.

## Reconnaissance
- `level13` is a SUID ELF 32-bit binary (not stripped).
- `ltrace` reveals a UID check:
```bash
getuid() = 2013
UID 2013 started us but we expect 4242
```
The program exits if the current UID is not `4242`.
`strings` also reveals:
```bash
your token is %s
```
Indicating the flag is printed only when the UID condition is satisfied.

## Exploitation
Using `gdb`, intercept the `getuid()` call and modify its return value:
```bash
gdb ./level13
break getuid
run
finish
set $eax = 4242
continue
```
This forces `getuid()` to return `4242`, bypassing the check and allowing execution to proceed.

## Flag
![Level13 Flag Screenshot](images/level13_flag.png)

