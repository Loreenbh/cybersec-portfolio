# Level00 – SnowCrash Write-up

## Objective
Obtain the flag for level00 by analyzing files belonging to the user `flag00`.

---

## Reconnaissance
Explored the filesystem for files owned by `flag00`:

```bash
find / -user "flag00" 2>/dev/null
