# 19 — Cross-Site Scripting (XSS)

> **Path Phase:** Web Attacks | **Difficulty:** Easy | **Sections:** 10

Finding and exploiting stored, reflected, and DOM XSS — defacing, phishing, session hijacking.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Intro to XSS
2. Stored XSS
3. Reflected XSS
4. DOM XSS
5. XSS Discovery
6. Defacing
7. Phishing
8. Session Hijacking
9. XSS Prevention
10. Skills Assessment

---

## 1 — Intro to XSS
XSS = injecting JavaScript that runs in a victim's browser.
```html
<script>alert(document.domain)</script>
```

## 2 — Stored XSS
Payload saved server-side (comments, profiles) → runs for every visitor.
```html
<script>alert('Stored XSS')</script>
```

## 3 — Reflected XSS
Payload reflected from request into response (search, error messages).
