# 🛡️ HTB CPTS — Penetration Tester Path Portfolio

> My complete study portfolio for the **Hack The Box Certified Penetration Testing Specialist (CPTS)** certification — all **28 modules** of the Penetration Tester job-role path, documented with notes, key concepts, tools, and skills-assessment writeups.

**Author:** Zeshan Ali · BS English, COMSATS University Islamabad
**Certification:** [HTB CPTS](https://academy.hackthebox.com/preview/certifications/htb-certified-penetration-testing-specialist)
**Path:** [Penetration Tester Job Role Path](https://academy.hackthebox.com/path/preview/penetration-tester)

---

## 📖 About This Repo

This repository is my hands-on documentation as I worked through the entire HTB CPTS path. Each module has its own folder containing a structured writeup: theory, key concepts, tools used, a walkthrough of the practical sections, and my approach to the skills assessment.

The CPTS path is organized to mirror the phases of a **real-world penetration test** — from reconnaissance and enumeration, through exploitation and privilege escalation, to lateral movement, Active Directory attacks, and professional reporting.

---

## 🗺️ Path Overview

- **Total Modules:** 28
- **Difficulty spread:** Easy (11), Fundamental (2), Medium (15)
- **Exam format:** Hands-on, real-world Active Directory network + commercial-grade report (no multiple choice)

---

## 📚 Modules

| # | Module | Phase | Difficulty | Sections |
|---|--------|-------|------------|----------|
| 01 | [Penetration Testing Process](./01-penetration-testing-process/) | `Introduction` | Fundamental | 15 |
| 02 | [Getting Started](./02-getting-started/) | `Introduction` | Fundamental | 23 |
| 03 | [Network Enumeration with Nmap](./03-network-enumeration-with-nmap/) | `Reconnaissance` | Easy | 12 |
| 04 | [Footprinting](./04-footprinting/) | `Reconnaissance` | Medium | 21 |
| 05 | [Information Gathering - Web Edition](./05-information-gathering-web-edition/) | `Reconnaissance` | Easy | 19 |
| 06 | [Vulnerability Assessment](./06-vulnerability-assessment/) | `Vulnerability Assessment` | Easy | 17 |
| 07 | [File Transfers](./07-file-transfers/) | `Post-Exploitation Utility` | Medium | 10 |
| 08 | [Shells & Payloads](./08-shells-payloads/) | `Exploitation` | Medium | 17 |
| 09 | [Using the Metasploit Framework](./09-using-the-metasploit-framework/) | `Exploitation` | Easy | 15 |
| 10 | [Password Attacks](./10-password-attacks/) | `Credential Attacks` | Medium | 26 |
| 11 | [Attacking Common Services](./11-attacking-common-services/) | `Exploitation` | Medium | 19 |
| 12 | [Pivoting, Tunneling, and Port Forwarding](./12-pivoting-tunneling-and-port-forwarding/) | `Post-Exploitation` | Medium | 18 |
| 13 | [Active Directory Enumeration & Attacks](./13-active-directory-enumeration-attacks/) | `Active Directory` | Medium | 36 |
| 14 | [Using Web Proxies](./14-using-web-proxies/) | `Web Tooling` | Easy | 15 |
| 15 | [Attacking Web Applications with Ffuf](./15-attacking-web-applications-with-ffuf/) | `Web Recon` | Easy | 13 |
| 16 | [Login Brute Forcing](./16-login-brute-forcing/) | `Web Attacks` | Easy | 13 |
| 17 | [SQL Injection Fundamentals](./17-sql-injection-fundamentals/) | `Web Attacks` | Medium | 17 |
| 18 | [SQLMap Essentials](./18-sqlmap-essentials/) | `Web Attacks` | Easy | 11 |
| 19 | [Cross-Site Scripting (XSS)](./19-cross-site-scripting-xss/) | `Web Attacks` | Easy | 10 |
| 20 | [File Inclusion](./20-file-inclusion/) | `Web Attacks` | Medium | 11 |
| 21 | [File Upload Attacks](./21-file-upload-attacks/) | `Web Attacks` | Medium | 11 |
| 22 | [Command Injections](./22-command-injections/) | `Web Attacks` | Medium | 12 |
| 23 | [Web Attacks](./23-web-attacks/) | `Web Attacks` | Medium | 18 |
| 24 | [Attacking Common Applications](./24-attacking-common-applications/) | `Application Attacks` | Medium | 33 |
| 25 | [Linux Privilege Escalation](./25-linux-privilege-escalation/) | `Privilege Escalation` | Easy | 28 |
| 26 | [Windows Privilege Escalation](./26-windows-privilege-escalation/) | `Privilege Escalation` | Medium | 33 |
| 27 | [Documentation & Reporting](./27-documentation-reporting/) | `Reporting` | Easy | 8 |
| 28 | [Attacking Enterprise Networks](./28-attacking-enterprise-networks/) | `Capstone` | Medium | 14 |

---

## 🧭 How the Path Is Structured

The 28 modules progress logically through a full engagement:

1. **Foundations** — pentest process, methodology, and getting started.
2. **Reconnaissance & Enumeration** — Nmap, footprinting services, web info gathering.
3. **Vulnerability Assessment** — scanning and prioritizing weaknesses.
4. **Exploitation** — shells, payloads, Metasploit, attacking services & web apps.
5. **Web Attacks** — the full web vulnerability suite (SQLi, XSS, file inclusion, uploads, command injection, and more).
6. **Post-Exploitation** — file transfers, pivoting, tunneling.
7. **Privilege Escalation** — Linux and Windows.
8. **Active Directory** — enumeration and attacks (the heart of the exam).
9. **Reporting** — documentation and a professional final report.
10. **Capstone** — Attacking Enterprise Networks ties it all together.

---

## 🧰 Core Toolset Across the Path

`Nmap` · `Burp Suite` · `Metasploit` · `ffuf` · `Hydra` · `Hashcat` / `John` · `SQLMap` · `BloodHound` · `Impacket` · `Chisel` / `Ligolo` · `Netcat` · `CrackMapExec` · `PowerView`

---

## ⚠️ Disclaimer

All techniques documented here were practiced in **Hack The Box Academy's authorized lab environment** for educational purposes. Nothing in this repository should be used against systems you do not have explicit permission to test.

---

## 📬 Connect

- **GitHub:** github.com/zeshanalioffical00-lab
- **Email:** zeshanalioffical00@gmail.com

---

*Built as part of my journey toward the HTB CPTS certification and a career in penetration testing.* 🚀
