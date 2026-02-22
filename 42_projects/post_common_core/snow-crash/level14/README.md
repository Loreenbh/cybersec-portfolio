# Level14 – SnowCrash

## Objective
Bypass anti-debugging and UID checks in the `getflag` SUID binary to retrieve the final token.

## Reconnaissance
- Target binary: `/bin/getflag` (ELF 32-bit, not stripped).
- `ltrace` reveals an anti-debugging mechanism using `ptrace()`.
Disassembly shows:
- A call to `ptrace()` to prevent tracing.
- A call to `getuid()` followed by comparisons against expected UIDs.
The program exits if either check fails.

## Exploitation
Using `gdb`, intercept and modify the return values of critical functions:
```bash
gdb /bin/getflag
```
Set breakpoints:
```bash
break ptrace
break getuid
run
```
1. Force `ptrace()` to return `0` to bypass anti-debugging:
```bash
finish
set $eax = 0
continue
```
2. Force `getuid()` to return the expected UID:
```bash
finish
set $eax = 3014
continue
```
This bypasses internal protections and allows the binary to print the flag.

## Flag
![Level14 Flag Screenshot](images/level14_flag.png)

