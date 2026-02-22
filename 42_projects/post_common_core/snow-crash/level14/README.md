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
- Force `ptrace()` to return `0` to bypass anti-debugging:
```bash
finish
set $eax = 0
continue
```
- Force `getuid()` to return the expected UID:
```bash
finish
set $eax = 3014
continue
```
- `eax` holds the function return value on x86.
- Overwriting it bypasses both security checks.
The binary proceeds normally and prints the final flag.

## Flag
![Level14 Flag Screenshot](images/level14_flag.png)

