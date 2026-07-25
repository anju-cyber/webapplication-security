# Nmap Cheat Sheet

A quick reference for commonly used Nmap commands.

---

# Host Discovery

Check if the host is online.

```bash
nmap -sn 192.168.52.129
```

---

# Basic TCP Scan

Scan the most common 1000 TCP ports.

```bash
nmap 192.168.52.129
```

---

# Scan Specific Ports

```bash
nmap -p 21,22,80 192.168.52.129
```

---

# Scan All TCP Ports

```bash
nmap -p- 192.168.52.129
```

---

# Service Version Detection

Identify software versions.

```bash
nmap -sV 192.168.52.129
```

---

# Operating System Detection

Attempt to identify the target OS.

```bash
nmap -O 192.168.52.129
```

---

# Default NSE Scripts

Run safe default scripts.

```bash
nmap -sC 192.168.52.129
```

---

# Aggressive Scan

Includes:

- OS Detection
- Version Detection
- Default NSE Scripts
- Traceroute

```bash
nmap -A 192.168.52.129
```

---

# UDP Scan

Scan UDP ports.

```bash
nmap -sU 192.168.52.129
```

---

# SYN Scan (Stealth Scan)

Requires root privileges.

```bash
sudo nmap -sS 192.168.52.129
```

---

# TCP Connect Scan

Used when SYN scan is unavailable.

```bash
nmap -sT 192.168.52.129
```

---

# Scan Speed

Slow Scan

```bash
nmap -T2 192.168.52.129
```

Normal Scan

```bash
nmap -T3 192.168.52.129
```

Fast Scan (Recommended for Lab)

```bash
nmap -T4 192.168.52.129
```

Very Aggressive

```bash
nmap -T5 192.168.52.129
```

---

# Verbose Output

```bash
nmap -v 192.168.52.129
```

Very Verbose

```bash
nmap -vv 192.168.52.129
```

---

# Save Scan Results

Normal Output

```bash
nmap -oN scan.txt 192.168.52.129
```

XML Output

```bash
nmap -oX scan.xml 192.168.52.129
```

Grepable Output

```bash
nmap -oG scan.gnmap 192.168.52.129
```

All Formats

```bash
nmap -oA fullscan 192.168.52.129
```

---

# NSE Script Scan

Run default scripts.

```bash
nmap -sC 192.168.52.129
```

Run a specific script.

```bash
nmap --script ftp-anon 192.168.52.129
```

Run vulnerability scripts.

```bash
nmap --script vuln 192.168.52.129
```

---

# Useful Combination

Service Detection + Default Scripts

```bash
nmap -sC -sV 192.168.52.129
```

Aggressive Scan with Faster Timing

```bash
sudo nmap -A -T4 192.168.52.129
```

Full TCP Port Scan + Version Detection

```bash
nmap -p- -sV 192.168.52.129
```

---

# Lab Target

| Machine | IP |
|---------|----|
| Kali Linux | 192.168.52.128 |
| Metasploitable2 | 192.168.52.129 |

---

**Note:** All commands in this cheat sheet were tested in my personal virtual lab against Metasploitable2 for learning purposes only.
