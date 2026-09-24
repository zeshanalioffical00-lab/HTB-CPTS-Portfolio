# 01 — Penetration Testing Process

> **Path Phase:** Introduction | **Difficulty:** Fundamental | **Sections:** 15

This module builds the mental framework for the entire path: what a penetration test actually is, the legal boundaries, and each phase of a professional engagement from pre-engagement paperwork to post-engagement cleanup. It is theory-heavy (no target machines), so this writeup documents the concepts you must know rather than commands.

---

## Table of Contents
1. Introduction to the Penetration Tester Path
2. Academy Modules Layout
3. Academy Exercises & Questions
4. Penetration Testing Overview
5. Laws and Regulations
6. Penetration Testing Process
7. Pre-Engagement
8. Information Gathering
9. Vulnerability Assessment
10. Exploitation
11. Post-Exploitation
12. Lateral Movement
13. Proof-of-Concept
14. Post-Engagement
15. Practice

---

## 4 — Penetration Testing Overview
**Key idea:** A penetration test is an authorized, simulated attack that evaluates security by finding and safely exploiting vulnerabilities. It differs from a vulnerability assessment (which only identifies) because it proves *impact* through exploitation.

## 5 — Laws and Regulations
**Must know:** You need **explicit written authorization** before any testing. Key laws: CFAA (US), Computer Misuse Act (UK), GDPR (EU). Always operate strictly within scope.

## 6 — Penetration Testing Process
Stages, performed roughly in order but often looping:

Pre-Engagement → Information Gathering → Vulnerability Assessment → Exploitation → Post-Exploitation → Lateral Movement → Proof-of-Concept → Post-Engagement

## 7 — Pre-Engagement
Paperwork and scoping stage. Key documents:

| Document | Purpose |
|----------|---------|
| **NDA** | Non-Disclosure Agreement — confidentiality |
| **SoW** | Scope of Work — what will be tested |
| **MSA** | Master Service Agreement — overarching terms |
| **RoE** | Rules of Engagement — how/when testing happens |

Test types: **Black Box** (no info), **Grey Box** (partial), **White Box** (full info).

## 8 — Information Gathering
Passive (OSINT) and active (scanning, enumeration) recon. Feeds every later stage.

## 9 — Vulnerability Assessment
Identify weaknesses — manual analysis + scanners (Nessus, OpenVAS). Prioritize by risk (CVSS).

## 10 — Exploitation
Executing attacks against confirmed vulnerabilities to gain a foothold.

## 11 — Post-Exploitation
Privilege escalation, sensitive data gathering, persistence, impact assessment.

## 12 — Lateral Movement
Pivoting from the compromised host to reach deeper internal systems.

## 13 — Proof-of-Concept (PoC)
Documenting a reproducible chain that proves the vulnerability is exploitable.

## 14 — Post-Engagement
Cleanup, report writing, delivery, and remediation retesting.

## 15 — Practice
Review section reinforcing the process end-to-end.

---

## 🔑 Key Takeaways
- Authorization and scope come **first** — everything else is illegal without them.
- The process is a **repeatable methodology**, not a checklist.
- Know the document types (NDA, SoW, MSA, RoE) — these appear on the exam.
- Every phase feeds the next; enumeration quality determines exploitation success.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
