# 05 — Information Gathering - Web Edition

> **Path Phase:** Reconnaissance | **Difficulty:** Easy | **Sections:** 19

Web-focused passive and active reconnaissance to map a target's attack surface.

> ℹ️ `{DOMAIN}` / `{TARGET}` = your target. Answers withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. WHOIS
3. Utilizing WHOIS
4. DNS
5. Digging DNS
6. Subdomains
7. Subdomain Bruteforcing
8. DNS Zone Transfers
9. Virtual Hosts
10. Certificate Transparency Logs
11. Fingerprinting
12. Crawling
13. robots.txt
14. .Well-Known URIs
15. Creepy Crawlies
16. Search Engine Discovery
17. Web Archives
18. Automating Recon
19. Skills Assessment

---

## 2–3 — WHOIS
```bash
whois {DOMAIN}
```

## 4–5 — DNS & Digging
```bash
dig {DOMAIN} A
dig {DOMAIN} MX
dig {DOMAIN} NS
dig {DOMAIN} TXT
dig @1.1.1.1 {DOMAIN} ANY
```

## 6–7 — Subdomains & Bruteforcing
```bash
curl -s "https://crt.sh/?q={DOMAIN}&output=json" | jq -r '.[].name_value' | sort -u
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u https://FUZZ.{DOMAIN}
gobuster dns -d {DOMAIN} -w <wordlist>
```

## 8 — DNS Zone Transfers
```bash
dig axfr {DOMAIN} @<nameserver>
```

## 9 — Virtual Hosts
```bash
gobuster vhost -u http://{TARGET} -w <wordlist> --append-domain
ffuf -w <wordlist> -u http://{TARGET} -H "Host: FUZZ.{DOMAIN}" -fs <baseline>
```

## 10 — Certificate Transparency
```bash
curl -s "https://crt.sh/?q=%.{DOMAIN}&output=json" | jq -r '.[].common_name' | sort -u
```

## 11 — Fingerprinting
```bash
whatweb {TARGET}
curl -I http://{TARGET}
wafw00f http://{TARGET}
nikto -h http://{TARGET}
```

## 12–15 — Crawling, robots.txt, .well-known
```bash
curl http://{TARGET}/robots.txt
curl http://{TARGET}/sitemap.xml
curl http://{TARGET}/.well-known/security.txt
```

## 16 — Search Engine Discovery (Dorking)                                                                                      site:{DOMAIN} inurl:admin
site:{DOMAIN} filetype:pdf
intitle:"index of" site:{DOMAIN}                                                                                                
## 17 — Web Archives
```bash
waybackurls {DOMAIN}
```

## 18 — Automating Recon
```bash
./FinalRecon.py --headers --whois --dns --sub --dir --url http://{TARGET}
```

## 19 — Skills Assessment
```bash
whois {DOMAIN}
dig axfr {DOMAIN} @<ns>
gobuster vhost -u http://{TARGET} -w <wordlist> --append-domain
whatweb {TARGET} && curl http://{TARGET}/robots.txt
```
> **Skills Assessment:** ✅ Completed in lab (answers withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `whois` / `dig` | Domain & DNS recon |
| `crt.sh` / `ffuf` / `gobuster` | Subdomain + vhost discovery |
| `whatweb` / `wafw00f` / `nikto` | Fingerprinting |
| `waybackurls` | Web archive endpoints |

## 🔑 Key Takeaways
- Certificate transparency (crt.sh) is the fastest passive subdomain source.
- Virtual host fuzzing finds sites that don't resolve via DNS.
- Always check `robots.txt`, `sitemap.xml`, `.well-known/`.
- Web archives reveal removed-but-working endpoints.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
