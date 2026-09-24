# 04 — Footprinting

> **Path Phase:** Reconnaissance | **Difficulty:** Medium | **Sections:** 21

Deep enumeration of every common infrastructure service — each has its own footprinting methodology.

> ℹ️ `{TARGET}` = target IP. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Enumeration Principles
2. Enumeration Methodology
3. Domain Information
4. Cloud Resources
5. Staff
6. FTP
7. SMB
8. NFS
9. DNS
10. SMTP
11. IMAP / POP3
12. SNMP
13. MySQL
14. MSSQL
15. Oracle TNS
16. IPMI
17. Linux Remote Management Protocols
18. Windows Remote Management Protocols
19. Footprinting Lab - Easy
20. Footprinting Lab - Medium
21. Footprinting Lab - Hard

---

## 1–5 — Principles, Methodology & OSINT
Layered approach: Infrastructure → Host → Services.
```bash
whois example.com
curl -s "https://crt.sh/?q=example.com&output=json" | jq -r '.[].name_value' | sort -u
```

## 6 — FTP (21)
```bash
ftp {TARGET}                     # anonymous
nmap --script ftp-anon,ftp-syst -p 21 {TARGET}
```

## 7 — SMB (139/445)
```bash
smbclient -N -L //{TARGET}
smbmap -H {TARGET}
rpcclient -U "" {TARGET}          # enumdomusers, querydominfo
enum4linux-ng -A {TARGET}
```

## 8 — NFS (111/2049)
```bash
showmount -e {TARGET}
sudo mount -t nfs {TARGET}:/<share> ./target-NFS
```

## 9 — DNS (53)
```bash
dig any example.com @{TARGET}
dig axfr example.com @{TARGET}    # zone transfer
dnsenum --dnsserver {TARGET} example.com
```

## 10 — SMTP (25)
```bash
nmap -p 25 --script smtp-commands,smtp-open-relay {TARGET}
smtp-user-enum -M VRFY -U users.txt -t {TARGET}
```

## 11 — IMAP/POP3 (143/993, 110/995)
```bash
openssl s_client -connect {TARGET}:993
openssl s_client -connect {TARGET}:995
```

## 12 — SNMP (161/UDP)
```bash
snmpwalk -v2c -c public {TARGET}
onesixtyone -c community.txt {TARGET}
braa <community>@{TARGET}:.1.3.6.*
```

## 13 — MySQL (3306)
```bash
mysql -u root -h {TARGET} -p
nmap -sV -p 3306 --script mysql-* {TARGET}
```

## 14 — MSSQL (1433)
```bash
mssqlclient.py <user>@{TARGET} -windows-auth
# enable_xp_cmdshell; xp_cmdshell whoami
```

## 15 — Oracle TNS (1521)
```bash
nmap -p 1521 --script oracle-sid-brute {TARGET}
odat all -s {TARGET}
sqlplus <user>/<pass>@{TARGET}/<SID>
```

## 16 — IPMI (623/UDP)
```bash
nmap -sU -p 623 --script ipmi-version {TARGET}
# msf: auxiliary/scanner/ipmi/ipmi_dumphashes → crack with hashcat -m 7300
```

## 17–18 — Linux / Windows Remote Management
```bash
ssh-audit {TARGET}
nmap -p 873 --script rsync-list-modules {TARGET}
evil-winrm -i {TARGET} -u <user> -p <pass>
xfreerdp /v:{TARGET} /u:<user> /p:<pass>
```

## 19–21 — Footprinting Labs (Easy/Medium/Hard)
```bash
nmap -sV -sC -p- {TARGET}
# → identify service → enumerate with commands above → find creds/flag
```
> **Labs (Easy/Medium/Hard):** ✅ Completed in lab (answers withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Category | Tools |
|----------|-------|
| SMB | `smbclient`, `smbmap`, `rpcclient`, `enum4linux-ng` |
| DNS | `dig`, `dnsenum` |
| SNMP | `snmpwalk`, `onesixtyone`, `braa` |
| DB | `mysql`, `mssqlclient.py`, `odat` |
| Windows | `evil-winrm`, `xfreerdp` |

## 🔑 Key Takeaways
- Every service has a **footprinting playbook** — learn the tool per service.
- Null sessions (SMB), anonymous logins (FTP), default creds are everywhere.
- SNMP `public` community string leaks huge amounts of info.
- Don't skip UDP — IPMI hash dumping (623) is a classic win.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
