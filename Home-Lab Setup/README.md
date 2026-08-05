# Project 01 — Advanced SOC Home Lab Setup 🏗️

## Objective
Build an enterprise-grade virtualized SOC lab environment with a dedicated firewall, attacker machine and defender machine — simulating a real corporate network architecture with proper network segmentation, traffic inspection and security monitoring capabilities.

---

## Lab Architecture

```
┌──────────────────────────────────────────────────────────────┐
│                        HOST MACHINE                          │
│                                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐  │
│  │   Kali Linux    │  │   OPNsense      │  │ Windows 11  │  │
│  │   VM            │→→│   Firewall      │→→│ VM          │  │
│  │   (Attacker)    │  │   (Gateway/IDS) │  │ (Defender)  │  │
│  │   4GB RAM       │  │   1GB RAM       │  │ 4GB RAM     │  │
│  │   2 Cores       │  │   1 Core        │  │ 2 Cores     │  │
│  └─────────────────┘  └─────────────────┘  └─────────────┘  │
│                                                              │
│  All traffic passes through OPNsense firewall                │
│  Suricata IDS monitors all network activity                  │
└──────────────────────────────────────────────────────────────┘
```

---

## Lab Upgrade — Why This Setup Is Better

| Feature | Old Lab | New Lab |
|---------|---------|---------|
| OS | Windows 10 | Windows 11 |
| Firewall | None | OPNsense (enterprise-grade) |
| IDS/IPS | None | Suricata (real-time detection) |
| RAM per VM | 2-3GB | 4GB each |
| Network | NAT + Host-Only | Internal Network + Bridged |
| Traffic logging | None | Full packet logging |
| Attack realism | Basic | Enterprise simulation |

---

## Host Machine Specifications

| Component | Details |
|-----------|---------|
| OS | [YOUR HOST OS] |
| RAM | [YOUR RAM] |
| CPU | [YOUR CPU] |
| Storage | [YOUR STORAGE] |
| Virtualization Software | VirtualBox |

---

## Virtual Machines

### 🔴 Kali Linux VM — Attacker

| Component | Details |
|-----------|---------|
| OS | Kali Linux (Latest) |
| RAM | 4GB |
| Storage | 50GB |
| CPU Cores | 2 |
| Role | Attacker — Offensive security tools, exploitation |
| Network | Internal Network → SOC_Lab |

### 🟡 OPNsense VM — Firewall/IDS

| Component | Details |
|-----------|---------|
| OS | OPNsense (FreeBSD based) |
| RAM | 1GB |
| Storage | 8GB |
| CPU Cores | 1 |
| Role | Perimeter firewall, IDS/IPS, traffic gateway |
| Network Adapter 1 | Bridged (WAN - internet access) |
| Network Adapter 2 | Internal Network → SOC_Lab (LAN) |

### 🔵 Windows 11 VM — Defender

| Component | Details |
|-----------|---------|
| OS | Windows 11 |
| RAM | 4GB |
| Storage | 100GB |
| CPU Cores | 2 |
| Role | Defender — Splunk SIEM, SOAR, target machine |
| Network | Internal Network → SOC_Lab |

---

## Network Architecture

```
Internet
    │
    │ (Bridged Adapter)
    ▼
┌─────────────────────────────────┐
│         OPNsense Firewall       │
│         WAN: [BRIDGED IP]       │
│         LAN: 10.0.2.1           │
│         Suricata IDS running    │
└─────────────────────────────────┘
    │
    │ (Internal Network - SOC_Lab)
    │
    ├──────────────────────────────
    │                             │
    ▼                             ▼
┌─────────────┐          ┌─────────────┐
│ Kali Linux  │          │ Windows 11  │
│ 10.0.2.85   │          │ 10.0.2.10   │
│ (Attacker)  │          │ (Defender)  │
└─────────────┘          └─────────────┘
```

---

## Network Configuration

| Setting | Value |
|---------|-------|
| Internal Network Name | SOC_Lab |
| OPNsense LAN | 10.0.2.1 |
| DHCP Range | 10.0.2.10 - 10.0.2.100 |
| Kali IP | 10.0.2.85 |
| Windows 11 IP | 10.0.2.10 |
| DNS | 8.8.8.8 (Google) |

---

## OPNsense Firewall Configuration

### Firewall Rules

| Rule | Action | Source | Destination | Port | Description |
|------|--------|--------|-------------|------|-------------|
| 1 | Pass | LAN network | any | SOC_Ports alias | Allow SOC lab services |
| 2 | Block | any | any | any | Default deny all |

### SOC_Ports Alias
```
80    → HTTP
443   → HTTPS
22    → SSH
23    → Telnet
21    → FTP
25    → SMTP
3389  → RDP
8000  → Splunk Web
8089  → Splunk Management
445   → SMB
```

### Suricata IDS — Custom Rules

```suricata
# Port Scan Detection
alert tcp any any -> $HOME_NET any (msg:"PORT SCAN DETECTED - Nmap SYN Scan"; flags:S; threshold:type threshold, track by_src, count 20, seconds 10; sid:1000001; rev:1;)

# SSH Brute Force Detection
alert tcp any any -> $HOME_NET 22 (msg:"SSH BRUTE FORCE ATTEMPT"; flow:to_server; threshold:type threshold, track by_src, count 5, seconds 60; sid:1000002; rev:1;)

# RDP Brute Force Detection
alert tcp any any -> $HOME_NET 3389 (msg:"RDP BRUTE FORCE ATTEMPT"; flow:to_server; threshold:type threshold, track by_src, count 5, seconds 60; sid:1000003; rev:1;)

# FTP Brute Force Detection
alert tcp any any -> $HOME_NET 21 (msg:"FTP BRUTE FORCE ATTEMPT"; flow:to_server; threshold:type threshold, track by_src, count 5, seconds 60; sid:1000004; rev:1;)

# ICMP Ping Sweep Detection
alert icmp any any -> $HOME_NET any (msg:"ICMP PING SWEEP DETECTED"; threshold:type threshold, track by_src, count 10, seconds 5; sid:1000005; rev:1;)
```

---

## Setup Process

### Step 1 — Install VirtualBox
- Downloaded VirtualBox from [virtualbox.org](https://www.virtualbox.org)
- Installed on host machine

### Step 2 — Create and Configure VMs
- Created three VMs with specified hardware allocations
- Configured network adapters for each VM

### Step 3 — Install OPNsense Firewall
- Downloaded OPNsense ISO from [opnsense.org](https://opnsense.org)
- Installed on OPNsense VM
- Configured WAN (Bridged) and LAN (Internal) interfaces
- Set static LAN IP: `10.0.2.1`
- Configured DHCP server for SOC_Lab network

### Step 4 — Configure Firewall Rules
- Created SOC_Ports alias with all lab service ports
- Applied least-privilege firewall rules
- Enabled default deny policy

### Step 5 — Configure Suricata IDS
- Enabled Suricata on LAN interface
- Enabled promiscuous mode for full traffic inspection
- Written custom detection rules for common attacks
- Verified alert generation via eve.json logs

### Step 6 — Configure Kali Linux
- Connected to SOC_Lab Internal Network
- Verified DHCP IP assignment from OPNsense
- Confirmed internet access through OPNsense

### Step 7 — Configure Windows 11
- Connected to SOC_Lab Internal Network
- Verified DHCP IP assignment from OPNsense
- Confirmed internet access through OPNsense
- Enabled OpenSSH Server for remote access testing

### Step 8 — Verify Full Lab Connectivity
```bash
# Kali → OPNsense
ping 10.0.2.1 ✅

# Kali → Windows 11
ping 10.0.2.10 ✅

# Windows 11 → OPNsense
ping 10.0.2.1 ✅

# Both VMs → Internet
ping 8.8.8.8 ✅
```

---

## IDS Alert Evidence

### SSH Brute Force Detection
```json
{
  "timestamp": "2026-08-02T17:44:11",
  "src_ip": "10.0.2.85",
  "dest_ip": "10.0.2.10",
  "dest_port": 22,
  "alert": {
    "signature": "SSH BRUTE FORCE ATTEMPT",
    "severity": 3
  }
}
```

---

## Key Improvements Over Previous Lab

1. **OPNsense firewall** — all traffic inspected before reaching Windows 11
2. **Suricata IDS** — real-time attack detection with custom rules
3. **4GB RAM per VM** — no more resource contention or crashes
4. **Windows 11** — more realistic modern enterprise target
5. **Proper network segmentation** — Internal Network isolates lab traffic
6. **Traffic logging** — eve.json logs feed into Splunk later

---

## Key Takeaways

1. **A firewall without IDS is incomplete** — you need both blocking and detection
2. **Custom IDS rules beat generic ones** — tuned to your specific lab scenarios
3. **Least privilege firewall rules** — only allow what's explicitly needed
4. **Network segmentation matters** — even in a lab environment
5. **4GB RAM makes everything smoother** — resource allocation directly impacts lab stability

---

## Screenshots
![VirtualBox Dashboard](./screenshots/virtualbox-dashboard.png)
![OPNsense Dashboard](./screenshots/opnsense-dashboard.png)
![Firewall Rules](./screenshots/firewall-rules.png)
![Suricata Alerts](./screenshots/suricata-alerts.png)
![Network Connectivity](./screenshots/network-connectivity.png)

---

## Next Project
➡️ [Project 02 — SIEM Setup with Splunk](../Project-02-SIEM-Splunk/)

---

*Part of my [SOC Analyst Portfolio](../README.md)*
