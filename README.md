## Tristen's Home Lab Security Operations Center

> Designed and documented a enterprise-grade Security Operations Center home lab with isolated VLANs, SIEM stack, and red/blue team capabilities. Aligned with CompTIA CySA+ CS0-003.


Overview

This repository documents the full design and architecture of my home lab Security Operations Center (SOC), built from the ground up to simulate a real-world enterprise security environment. The goal is to generate realistic attack traffic, detect it using industry-standard SIEM tools, and investigate it the same way a professional SOC analyst would.

This project serves as the foundation for an independent study in cybersecurity operations and maps directly to all four domains of the CompTIA CySA+ CS0-003 certification exam.


##Network Architecture

The lab is segmented into four isolated VLANs, each with its own purpose, security policy, and software stack. All inter-VLAN routing is handled by a Netgate 2100 running pfSense+, and all traffic is mirrored to the SIEM via a SPAN port on the UniFi USW-Pro-Max-16-PoE.

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | Management | 10.0.10.0/24 | Admin access only. Hosts pfSense WebGUI, UniFi Controller, Proxmox |
| 20 | SIEM / Monitoring | 10.0.20.0/24 | Security Onion, Wazuh, Suricata, ELK Stack |
| 30 | Victim Zone | 10.0.30.0/24 | Windows Server 2022 AD, Windows 10 Pro, Metasploitable 3, DVWA |
| 40 | Attack Zone | 10.0.40.0/24 | Kali Linux, MITRE Caldera, Sliver C2, offensive Windows VM |


## Attack Zone

The attack zone simulates a real-world threat actor operating against a corporate environment.

Tools:
- **Kali Linux** — Primary attack platform. Includes Metasploit, Burp Suite, Wireshark, Hydra, Nmap, RustScan
- **MITRE Caldera** — Automated adversary emulation using MITRE ATT&CK techniques
- **Sliver C2** — Command and control framework for post-exploitation simulation
- **Nmap / RustScan** — Network reconnaissance and port scanning


## Victim Zone

The victim zone simulates a realistic corporate environment with intentionally vulnerable systems.

Systems:
- **Windows Server 2022** — Active Directory Domain Controller. Target for credential attacks, privilege escalation, and lateral movement
- **Windows 10 Pro** — Simulated employee workstation with Wazuh agent installed
- **Metasploitable 3** — Intentionally vulnerable Linux/Windows VMs for exploitation practice
- **DVWA** — Damn Vulnerable Web Application for web app penetration testing (SQLi, XSS, CSRF, command injection)

All VMs are hosted on Proxmox running on a custom Ryzen 5 7600X build with 32GB RAM and a 2TB NVMe SSD.


## SIEM / Monitoring Zone

The monitoring zone captures, analyzes, and alerts on all traffic across the lab via SPAN port mirroring.

Tools:
- **Security Onion** — Full SOC platform. Captures and analyzes all mirrored SPAN traffic
- **Wazuh** — Endpoint Detection and Response (EDR). Monitors all physical and virtual devices
- **Suricata** — Rule-based IDS/IPS. Analyzes traffic for known attack signatures and anomalies
- **ELK Stack** — Elasticsearch, Logstash, Kibana for centralized log aggregation and dashboards
- **Nessus** — Scheduled vulnerability scanning of the victim zone


## Management Zone

The management zone is the most locked-down VLAN in the lab. Only trusted admin interfaces and tools are allowed.

Tools:
- **pfSense+ WebGUI** — Firewall rule management, VLAN configuration, DHCP, NAT, VPN
- **UniFi Network Controller** — Switch port management, VLAN tagging, SPAN mirror configuration
- **Proxmox WebGUI** — VM deployment, snapshots, resource allocation
- **JetKVM** — Out-of-band remote access at the BIOS level for headless server management
- **PuTTY / Terminal** — Console access to all devices


## Hardware

| Device | Specs / Model | Role |
|--------|--------------|------|
| Attack Machine | Lenovo Legion 7i | Runs Kali Linux and attack tools |
| SIEM / Proxmox Host | Ryzen 5 7600X, RTX 5070, 32GB RAM, 2TB SSD | Hosts all VMs, runs SIEM stack |
| Firewall | Netgate 2100 (pfSense+) | Inter-VLAN routing, firewall rules, VPN |
| Switch | UniFi USW-Pro-Max-16-PoE | VLAN tagging, SPAN port mirroring |
| UPS | CyberPower CP1500PFCRM2U | Battery backup and surge protection |
| KVM | JetKVM | Out-of-band BIOS-level access |
| Server Rack | Wall-mounted rack enclosure | Houses firewall, switch, UPS, patch panel |
| Displays | 49" Ultrawide + 32" Vertical + Legion 7i | Multi-pane SOC monitoring workspace |


## CySA+ CS0-003 Domain Alignment

This project maps to all four CySA+ exam domains:

| Domain | Coverage |
|--------|----------|
| 1 - Security Operations | SIEM monitoring, log analysis, alert triage |
| 2 - Vulnerability Management | Nessus scanning, patch management, risk prioritization |
| 3 - Incident Response | Attack simulation, detection, investigation, documentation |
| 4 - Reporting & Communication | Written investigation reports for every simulated attack |


## Stretch Goals

- [ ] AlienVault OTX threat intelligence integration script
- [ ] SOAR playbook using MISP and the pfSense API
- [ ] Grafana SOC dashboard on a rack-mounted display
- [ ] Python script to auto-correlate Wazuh alerts with ATT&CK techniques


## Status

> Currently in planning and equipment acquisition phase.
> Hardware has been purchased. Lab build in progress.
> Documentation and architecture design complete.


*Built by Tristen Dennis*
