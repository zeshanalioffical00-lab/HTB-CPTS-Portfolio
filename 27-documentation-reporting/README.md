# 27 — Documentation & Reporting

> **Path Phase:** Reporting | **Difficulty:** Easy | **Sections:** 8

Professional pentest documentation and writing a commercial-grade report — a graded part of the CPTS exam.

---

## Table of Contents
1. Introduction to Documentation & Reporting
2. Notetaking & Organization
3. Types of Reports
4. Components of a Report
5. How to Write Up a Finding
6. Reporting Tips and Tricks
7. Practice Lab
8. Beyond this Module

---

## 1 — Introduction
Documentation runs through the **entire** engagement, not just the end. Log every command, screenshot, and credential with timestamps for a reproducible, defensible report.

## 2 — Notetaking & Organization
Tools: CherryTree, Obsidian, Notion, OneNote, Sysreptor. Organize by host / service / finding / credentials.
```bash
script engagement_log.txt      # log terminal sessions
```

## 3 — Types of Reports
| Report | Audience |
|--------|----------|
| **Executive Summary** | Management — business risk, no jargon |
| **Technical Report** | IT/Security — full findings + reproduction |
| **Attestation** | Compliance/3rd parties — proof of test |
| **Remediation** | Fix-focused follow-up |

## 4 — Components of a Report
1. Cover page & confidentiality notice
2. Executive Summary
3. Scope & methodology
4. Findings (with severity/CVSS)
5. Attack narrative / chain
6. Remediation recommendations
7. Appendices (evidence, tools, raw output)

## 5 — How to Write Up a Finding ⭐
Each finding needs: **Title, Severity/CVSS, Description, Affected assets, Evidence/PoC, Impact, Remediation, References.**
```markdown
### Finding: SQL Injection in Login Form
**Severity:** Critical (CVSS 9.8)
**Affected:** http://target/login.php (parameter: username)
**Description:** The username parameter is not sanitized...
**Steps to Reproduce:**
1. Send `admin' OR 1=1-- -` in username... [screenshot]
**Impact:** Full database compromise, authentication bypass.
**Remediation:** Use parameterized queries / prepared statements.
**References:** CWE-89, OWASP A03:2021
```

## 6 — Reporting Tips
- Write for the audience (exec vs technical).
- Be objective and factual — no hype.
- Screenshots legible; redact sensitive data.
- Rate severity consistently (CVSS).
- Provide actionable, prioritized remediation.

## 7 — Practice Lab
Take provided findings/data → produce a structured report following the components above.
> **Practice Lab:** ✅ Completed in lab (documented via methodology above)

## 8 — Beyond this Module
Report templates, Sysreptor automation, refining your own reporting workflow for the exam.

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| Obsidian / CherryTree / Sysreptor | Notetaking & report generation |
| `script` | Terminal session logging |
| CVSS calculator | Severity scoring |

## 🔑 Key Takeaways
- **The CPTS exam grades your report** — this module is not optional.
- Every finding needs reproducible steps + evidence + remediation.
- Separate executive (business) from technical (IT) audiences.
- Document *as you go* — reconstructing later loses evidence.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
