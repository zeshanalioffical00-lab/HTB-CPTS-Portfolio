# 15 — Attacking Web Applications with Ffuf

> **Path Phase:** Web Recon | **Difficulty:** Easy | **Sections:** 13

Web content discovery and fuzzing with `ffuf` — directories, files, parameters, subdomains, vhosts.

> ℹ️ `{TARGET}` / `{DOMAIN}` = target. Answers withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. Web Fuzzing
3. Directory Fuzzing
4. Page Fuzzing
5. Recursive Fuzzing
6. DNS Records
7. Sub-domain Fuzzing
8. Vhost Fuzzing
9. Filtering Results
10. Parameter Fuzzing - GET
11. Parameter Fuzzing - POST
12. Value Fuzzing
13. Skills Assessment

---

## 2–3 — Web / Directory Fuzzing
`FUZZ` is the keyword ffuf replaces with each wordlist entry.
```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://{TARGET}/FUZZ
```

## 4 — Page Fuzzing (extensions)
```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://{TARGET}/indexFUZZ
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/FUZZ.php
```

## 5 — Recursive Fuzzing
```bash
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/FUZZ -recursion -recursion-depth 1 -e .php -v
```

## 6–7 — DNS / Sub-domain Fuzzing
```bash
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.{DOMAIN}/
```

## 8 — Vhost Fuzzing
```bash
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/ -H "Host: FUZZ.{DOMAIN}" -fs <baseline-size>
```

## 9 — Filtering Results
```bash
# Filter by size (-fs), words (-fw), lines (-fl), status (-fc); match with -mc
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/FUZZ -fc 404 -fs 4242
```

## 10 — Parameter Fuzzing (GET)
```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://{TARGET}/script.php?FUZZ=key -fs <size>
```

## 11 — Parameter Fuzzing (POST)
```bash
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/script.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs <size>
```

## 12 — Value Fuzzing
```bash
ffuf -w ids.txt:FUZZ -u http://{TARGET}/script.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs <size>
```

## 13 — Skills Assessment
```bash
ffuf -w <wordlist>:FUZZ -u http://{TARGET}:PORT/FUZZ -recursion -e .php
ffuf -w <wordlist>:FUZZ -u http://{TARGET}:PORT/ -H "Host: FUZZ.{DOMAIN}" -fs <size>
# → parameter + value fuzzing on the found page
```
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `ffuf` | Directory, page, param, subdomain, vhost fuzzing |
| SecLists | Wordlists |

## 🔑 Key Takeaways
- `FUZZ` keyword placement controls what gets fuzzed.
- **Filtering (`-fs/-fc/-fw`)** is the real skill — baseline first, then filter noise.
- Recursive fuzzing finds nested content automatically.
- Vhost fuzzing needs a size filter against the default response.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
