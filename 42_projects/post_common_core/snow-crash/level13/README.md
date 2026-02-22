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
- The program exits if the current UID is not `4242`.
`strings` reveals:
```bash
your token is %s
```
This indicates that the flag is printed only when the UID condition is satisfied.

### Vulnerability
- The binary relies solely on the return value of `getuid()` to enforce access control.
- This check can be bypassed by modifying the return value at runtime using a debugger.

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
- break `getuid` sets a breakpoint on the UID check.
- After the function returns, the `eax` register (return value) is modified to `4242`.
The program proceeds as if it were executed by UID `4242`, revealing the flag.

## Flag
![Level13 Flag Screenshot](images/level13_flag.png)

