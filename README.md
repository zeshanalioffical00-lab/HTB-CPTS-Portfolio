# 🛡️ HTB CPTS — Penetration Tester Path Portfolio

> My complete hands-on portfolio for the **Hack The Box Certified Penetration Testing Specialist (CPTS)** — all **28 modules** of the Penetration Tester job-role path, documented with theory, commands, tools, and skills-assessment methodology.

<p align="center">
  <img src="https://img.shields.io/badge/Certification-HTB%20CPTS-9FEF00?style=for-the-badge&logo=hackthebox&logoColor=black" />
  <img src="https://img.shields.io/badge/Modules-28-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-Penetration%20Testing-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Status-In%20Progress-yellow?style=for-the-badge" />
</p>

**Author:** Zeshan Ali · BS English, COMSATS University Islamabad
**Certification:** [HTB CPTS](https://academy.hackthebox.com/preview/certifications/htb-certified-penetration-testing-specialist) · **Path:** [Penetration Tester Job-Role Path](https://academy.hackthebox.com/path/preview/penetration-tester)

---

## 📖 About This Repo

This repository is my hands-on documentation of the entire HTB CPTS path. Each module has its own folder with a structured writeup: **theory & key concepts**, the **exact tools and commands** used, a walkthrough of the practical sections, and my **methodology for the skills assessment**.

The path mirrors the phases of a **real-world penetration test** — from reconnaissance and enumeration, through exploitation and privilege escalation, to lateral movement, Active Directory attacks, and professional reporting.

> ⚠️ **Note on flags:** In line with HTB Academy's policy, exam/lab **flags and direct answers are intentionally withheld**. This portfolio documents *methodology, commands, and understanding* — not answer keys.

---

## 🗺️ Path Overview

| | |
|---|---|
| **Total Modules** | 28 |
| **Difficulty Spread** | 🟢 Easy (11) · 🔵 Medium (15) · ⚪ Fundamental (2) |
| **Estimated Time** | ~42 days (8 hrs/day) |
| **Exam Format** | Hands-on, real-world Active Directory network + commercial-grade report (**no multiple choice**) |

---

## 📚 Modules

### 🧭 Phase 1 — Foundations
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 01 | [Penetration Testing Process](./01-penetration-testing-process/) | Fundamental | 15 |
| 02 | [Getting Started](./02-getting-started/) | Fundamental | 23 |

### 🔍 Phase 2 — Reconnaissance & Enumeration
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 03 | [Network Enumeration with Nmap](./03-network-enumeration-with-nmap/) | Easy | 12 |
| 04 | [Footprinting](./04-footprinting/) | Medium | 21 |
| 05 | [Information Gathering - Web Edition](./05-information-gathering-web-edition/) | Easy | 19 |
| 06 | [Vulnerability Assessment](./06-vulnerability-assessment/) | Easy | 17 |

### 💥 Phase 3 — Exploitation & Services
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 07 | [File Transfers](./07-file-transfers/) | Medium | 10 |
| 08 | [Shells & Payloads](./08-shells-payloads/) | Medium | 17 |
| 09 | [Using the Metasploit Framework](./09-using-the-metasploit-framework/) | Easy | 15 |
| 10 | [Password Attacks](./10-password-attacks/) | Medium | 26 |
| 11 | [Attacking Common Services](./11-attacking-common-services/) | Medium | 19 |

### 🌐 Phase 4 — Web Attacks
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 14 | [Using Web Proxies](./14-using-web-proxies/) | Easy | 15 |
| 15 | [Attacking Web Apps with Ffuf](./15-attacking-web-applications-with-ffuf/) | Easy | 13 |
| 16 | [Login Brute Forcing](./16-login-brute-forcing/) | Easy | 13 |
| 17 | [SQL Injection Fundamentals](./17-sql-injection-fundamentals/) | Medium | 17 |
| 18 | [SQLMap Essentials](./18-sqlmap-essentials/) | Easy | 11 |
| 19 | [Cross-Site Scripting (XSS)](./19-cross-site-scripting-xss/) | Easy | 10 |
| 20 | [File Inclusion](./20-file-inclusion/) | Medium | 11 |
| 21 | [File Upload Attacks](./21-file-upload-attacks/) | Medium | 11 |
| 22 | [Command Injections](./22-command-injections/) | Medium | 12 |
| 23 | [Web Attacks (Verb Tampering, IDOR, XXE)](./23-web-attacks/) | Medium | 18 |
| 24 | [Attacking Common Applications](./24-attacking-common-applications/) | Medium | 33 |

### 🔀 Phase 5 — Post-Exploitation & Privilege Escalation
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 12 | [Pivoting, Tunneling & Port Forwarding](./12-pivoting-tunneling-and-port-forwarding/) | Medium | 18 |
| 25 | [Linux Privilege Escalation](./25-linux-privilege-escalation/) | Easy | 28 |
| 26 | [Windows Privilege Escalation](./26-windows-privilege-escalation/) | Medium | 33 |

### 🏰 Phase 6 — Active Directory (Heart of the Exam)
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 13 | [Active Directory Enumeration & Attacks](./13-active-directory-enumeration-attacks/) | Medium | 36 |

### 📝 Phase 7 — Reporting & Capstone
| # | Module | Difficulty | Sections |
|---|--------|------------|----------|
| 27 | [Documentation & Reporting](./27-documentation-reporting/) | Easy | 8 |
| 28 | [Attacking Enterprise Networks](./28-attacking-enterprise-networks/) | Medium | 14 |

---

## 🧰 Toolset (by category)

**🔍 Recon & Enumeration**
`Nmap` · `Masscan` · `ffuf` · `gobuster` · `feroxbuster` · `dig` · `dnsenum` · `dnsrecon` · `whatweb` · `wafw00f` · `nikto` · `enum4linux-ng` · `smbmap` · `smbclient` · `rpcclient` · `snmpwalk` · `onesixtyone` · `crt.sh` · `waybackurls`

**💥 Exploitation**
`Metasploit` · `msfvenom` · `searchsploit` · `Burp Suite` · `OWASP ZAP` · `sqlmap` · `XSStrike` · `wpscan` · `droopescan` · `commix`

**🔑 Credential Attacks**
`Hydra` · `Medusa` · `Hashcat` · `John the Ripper` · `CrackMapExec` / `NetExec` · `cewl` · `username-anarchy` · `*2john` suite

**🏰 Active Directory**
`BloodHound` / `SharpHound` · `Impacket` (secretsdump, GetUserSPNs, psexec, ntlmrelayx) · `Responder` · `Inveigh` · `kerbrute` · `Rubeus` · `mimikatz` · `PowerView` · `Certipy` · `evil-winrm` · `ldapsearch`

**🔀 Pivoting & Post-Exploitation**
`Chisel` · `Ligolo-ng` · `sshuttle` · `proxychains` · `socat` · `plink.exe` · `linPEAS` · `winPEAS` · `pspy` · `PowerUp` · `LaZagne` · `GTFOBins` · `LOLBAS`

**🪟 Privilege Escalation**
`PrintSpoofer` · `GodPotato` · `JuicyPotatoNG` · `accesschk` · `Watson` · `Seatbelt` · GTFOBins / LOLBAS references

**📝 Reporting**
`Sysreptor` · `Obsidian` · `CherryTree` · CVSS calculator

---

## 🧭 The Engagement Flow (how it all connects)

Recon → Enumeration → Vulnerability Assessment → Exploitation →
Foothold → Privilege Escalation → Pivoting → Lateral Movement →
Active Directory Compromise → Domain Admin → Reporting


Every module plugs into this chain — which is exactly what the CPTS exam tests end-to-end.

---

## ⚠️ Disclaimer

All techniques documented here were practiced in **Hack The Box Academy's authorized lab environment** for educational purposes only. Nothing in this repository should be used against systems you do not have **explicit written permission** to test.

---

## 📬 Connect

- **GitHub:** [github.com/zeshanalioffical00-lab](https://github.com/zeshanalioffical00-lab)
- **Email:** zeshanalioffical00@gmail.com

---

<p align="center"><i>Built as part of my journey toward the HTB CPTS certification and a career in penetration testing.</i> 🚀</p>
<p align="center">⭐ If you find this helpful, consider starring the repo!</p>
