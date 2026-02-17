# Level02 – SnowCrash

## Objective
Recover the credentials transmitted over the network and obtain the flag by analyzing an intercepted Telnet session with Wirehark.

## Reconnaissance
Found the file `level02.pcap` owned by `flag02`:
```bash
ls -la
----r--r-- 1 flag02 level02 8302 Aug 30 2015 level02.pcap
```
This file contains network traffic to be analyzed.

## Exploitation

- Transfer  the file to the host machine
```bash
scp -P 4243 level02@10.14.14.4:/home/user/level02/level02.pcap ~/Downloads/
```
- Analyze the capture in Wireshark.
- Follow the TCP stream of the Telnet session to extract credentials.
- The passwords contains unwanted control characters(0x7f)

![Level02 Wireshark screenshot](images/wireshark_passwd.png)

## Flag

![Level02 Flag Screenshot](images/level02_flag.png)
