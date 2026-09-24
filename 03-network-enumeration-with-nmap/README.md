# 03 — Network Enumeration with Nmap

> **Path Phase:** Reconnaissance | **Difficulty:** Easy | **Sections:** 12

Nmap is the backbone of enumeration: host discovery, port scanning, service/version detection, NSE, tuning, and firewall/IDS evasion.

> ℹ️ `{TARGET}` = your spawned box IP. Flags withheld per HTB Academy policy.

---

## Table of Contents
1. Introduction to Nmap
2. Host Discovery
3. Host and Port Scanning
4. Saving the Results
5. Service Enumeration
6. Nmap Scripting Engine
7. Performance
8. Firewall and IDS/IPS Evasion
9. Skills Assessment

---

## 1 — Introduction to Nmap
| Flag | Scan Type |
|------|-----------|
| `-sS` | TCP SYN (stealth) |
| `-sT` | TCP Connect |
| `-sU` | UDP scan |
| `-sn` | Ping (host discovery only) |
| `-A` | Aggressive (OS + version + script + traceroute) |

## 2 — Host Discovery
```bash
nmap -sn -oA host_discovery {TARGET}
nmap -sn -oA tnet 10.129.2.0/24 | grep for | cut -d" " -f5
sudo nmap {TARGET} -sn -oA host -PE --packet-trace
```

## 3 — Host and Port Scanning
```bash
sudo nmap {TARGET} -p 21 --packet-trace -Pn -n --disable-arp-ping
sudo nmap -p- {TARGET}
sudo nmap --top-ports=100 {TARGET}
sudo nmap -sU -p 137,138,139 {TARGET}
```

## 4 — Saving the Results
```bash
nmap {TARGET} -p- -oA target_full
xsltproc target_full.xml -o target_full.html
```
`-oN` Normal · `-oG` Greppable · `-oX` XML · `-oA` All

## 5 — Service Enumeration
```bash
sudo nmap {TARGET} -p- -sV -oA service_scan
nc -nv {TARGET} <port>   # manual banner grab
```

## 6 — Nmap Scripting Engine (NSE)
```bash
sudo nmap {TARGET} -sC
sudo nmap {TARGET} -p 25 --script banner
sudo nmap {TARGET} --script vuln
sudo nmap {TARGET} -p- -A -oA aggressive_scan
```

## 7 — Performance
```bash
sudo nmap {TARGET} -p- -T4 --min-rate=1000
sudo nmap {TARGET} -p- --max-retries 1 --initial-rtt-timeout 50ms
```

## 8 — Firewall and IDS/IPS Evasion
```bash
sudo nmap {TARGET} -sA -p 21,22,25 -Pn      # detect filtering
sudo nmap {TARGET} -sS -Pn -f               # fragment packets
sudo nmap {TARGET} -sS -Pn -D RND:5         # decoys
sudo nmap {TARGET} -sS -Pn -g 53            # spoof source port
sudo nmap {TARGET} -sS -Pn --data-length 200
```

## 9 — Skills Assessment
```bash
sudo nmap -sn {TARGET}
sudo nmap -p- -sS -Pn -n --source-port 53 -f {TARGET} -oA sa_full
sudo nmap -sV -sC -p <found_ports> --source-port 53 {TARGET} -oA sa_services
nc -nv --source-port 53 {TARGET} <port>
```
> **Skills Assessment:** ✅ Completed in lab (flags withheld per HTB Academy policy)

---

## 🧰 Tools Used
| Tool | Purpose |
|------|---------|
| `nmap` | Host discovery, port/service scanning, NSE |
| `netcat` | Manual banner grabbing |
| `xsltproc` | XML → HTML report |

## 🔑 Key Takeaways
- Always run a **full port scan** (`-p-`) — services hide on non-standard ports.
- **Service versions** (`-sV`) bridge enumeration to exploitation (CVE lookup).
- Evasion with `--source-port 53`, fragmentation, and decoys works on filtered targets.
- Save everything with `-oA` for your report.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
