# Nmap Methodology

This is the methodology I followed while learning Nmap.

---

## Step 1 — Verify Host Availability

Before scanning ports, confirm that the target system is online.

Example:

```bash
nmap -sn 192.168.52.129
```

Purpose

- Check whether the target is alive.
- Avoid scanning an offline system.

---

## Step 2 — Basic Port Scan

Identify open TCP ports.

```bash
nmap 192.168.52.129
```

Purpose

- Discover open ports
- Identify exposed services

---

## Step 3 — Service Version Detection

Identify software versions running on each service.

```bash
nmap -sV 192.168.52.129
```

Purpose

- Determine exact software versions.
- Useful for vulnerability research.

---

## Step 4 — Operating System Detection

Attempt to identify the target operating system.

```bash
nmap -O 192.168.52.129
```

Purpose

- Detect the operating system.
- Helps understand the target environment.

---

## Step 5 — Default NSE Scan

Run default Nmap scripts.

```bash
nmap -sC 192.168.52.129
```

Purpose

- Gather additional information.
- Detect common misconfigurations.
- Perform safe enumeration.

---

## Step 6 — Aggressive Scan

Run an aggressive scan combining multiple detection techniques.

```bash
nmap -A 192.168.52.129
```

Purpose

- Service Detection
- Version Detection
- OS Detection
- Traceroute
- Default NSE Scripts

---

## Step 7 — UDP Scan

Identify open UDP services.

```bash
nmap -sU 192.168.52.129
```

Purpose

- Discover UDP services.
- Identify services such as DNS, SNMP, NTP, etc.

---

## Step 8 — Performance Optimization

Increase scan speed when required.

```bash
nmap -T4 192.168.52.129
```

Purpose

- Faster scanning
- Suitable for lab environments

---

## Step 9 — Save Scan Results

Store results for future analysis.

```bash
nmap -oN normal.txt 192.168.52.129
nmap -oX scan.xml 192.168.52.129
nmap -oG scan.gnmap 192.168.52.129
nmap -oA fullscan 192.168.52.129
```

Purpose

- Documentation
- Reporting
- Import into other tools

---

## Final Analysis

After completing the scan:

- Identify exposed services
- Review service versions
- Look for anonymous access
- Check weak configurations
- Research known vulnerabilities
- Plan further enumeration
