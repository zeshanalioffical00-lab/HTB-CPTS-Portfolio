# 14 — Using Web Proxies

> **Path Phase:** Web Tooling | **Difficulty:** Easy | **Sections:** 15

Mastering Burp Suite and OWASP ZAP — the core tools for all web application testing.

> ℹ️ `{TARGET}` = target. Answers withheld per HTB Academy policy.

---

## Table of Contents
1. Intro to Web Proxies
2. Setting Up
3. Proxy Setup
4. Intercepting Web Requests
5. Intercepting Responses
6. Automatic Modification
7. Repeating Requests
8. Encoding/Decoding
9. Proxying Tools
10. Burp Intruder
11. ZAP Fuzzer
12. Burp Scanner
13. ZAP Scanner
14. Extensions
15. Skills Assessment

---

## 1–3 — Setup & Proxy Config  
Browser → FoxyProxy → 127.0.0.1:8080
Visit http://burp → download & install CA cert  

## 4–5 — Intercepting Requests & Responses
Burp **Proxy → Intercept**: capture, edit, forward requests. Enable "Response interception" to modify server responses (unhide fields, bypass client-side checks).

## 6 — Automatic Modification
Proxy → Options → Match & Replace rules (auto-change headers/params on every request).

## 7 — Repeating Requests (Repeater)
Send request to **Repeater** (Ctrl+R) → modify → resend → analyze. The core manual-testing workflow.

## 8 — Encoding/Decoding
Burp **Decoder** / ZAP: URL, Base64, HTML, hex, hashing — essential for crafting payloads.

## 9 — Proxying Tools
```bash
sqlmap -u "http://{TARGET}/?id=1" --proxy=http://127.0.0.1:8080
curl -x http://127.0.0.1:8080 http://{TARGET}
```

## 10 — Burp Intruder
Automated fuzzing. Attack types: Sniper, Battering ram, Pitchfork, Cluster bomb. Set payload positions (§) → load wordlist → attack → filter by status/length.
> **Intruder result:** ✅ Completed in lab (withheld per HTB Academy policy)

## 11 — ZAP Fuzzer
Right-click request → Fuzz → add payloads (ZAP's Intruder equivalent).

## 12–13 — Burp / ZAP Scanner
Active + passive scanning for common vulns (XSS, SQLi). Burp Pro / ZAP automated scan.

## 14 — Extensions
BApp Store: Logger++, Autorize, JSON Web Tokens, ActiveScan++. ZAP marketplace add-ons.

## 15 — Skills Assessment                                                      
Intercept the request, modify hidden params, use Repeater to manipulate
values, Decoder/Intruder to find the flag.  
> **Skills Assessment:** ✅ Completed in lab (answer withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| Burp Suite | Intercept, Repeater, Intruder, Decoder, Scanner |
| OWASP ZAP | Free alternative — Fuzzer, Scanner |
| FoxyProxy | Browser proxy switching |

## 🔑 Key Takeaways
- **Repeater** is where most manual web testing happens — live in it.
- Intercept responses to bypass client-side restrictions.
- Proxy other tools (sqlmap, curl) through Burp for visibility.
- Decoder handles all the encoding you'll need for payloads.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
Configure browser (FoxyProxy) → 127.0.0.1:8080. Install Burp/ZAP CA cert to intercept HTTPS.
