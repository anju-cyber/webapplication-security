# Nmap Commands

---

## Host Discovery

```bash
nmap -sn 192.168.52.129
```

Purpose

Checks whether the target is online.

---

## Basic Scan

```bash
nmap 192.168.52.129
```

Purpose

Discovers open TCP ports.

---

## Service Detection

```bash
nmap -sV 192.168.52.129
```

Purpose

Identifies service versions.

---

## Default Scripts

```bash
nmap -sC 192.168.52.129
```

Purpose

Runs default NSE scripts.

---

## OS Detection

```bash
nmap -O 192.168.52.129
```

Purpose

Attempts to identify the operating system.

---

## Aggressive Scan

```bash
nmap -A 192.168.52.129
```

Purpose

Runs multiple detection techniques together.

---

## UDP Scan

```bash
nmap -sU 192.168.52.129
```

Purpose

Scans UDP ports.

---

## Faster Scan

```bash
nmap -T4 192.168.52.129
```

Purpose

Speeds up scanning.

---

## Save Output

```bash
nmap -oA fullscan 192.168.52.129
```

Purpose

Saves scan results in Normal, XML and Grepable formats.
