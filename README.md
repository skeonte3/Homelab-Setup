# Home Lab Setup

## Overview
**Hypervisor:** Proxmox VE 8.4  
**Hardware:** Dell OptiPlex 7020  
**Purpose:** Self-hosted homelab for cybersecurity learning, SOC analyst practice, and home server management.  
**Objective:** Build a real-world environment to practice defensive security, threat detection, network monitoring, and self-hosted services.

---

## Hardware Specs

| Component | Details |
|-----------|---------|
| Machine | Dell OptiPlex 7020 |
| Hypervisor | Proxmox VE 8.4 |
| Network | Ethernet to Spectrum router |
| Management IP | 192.168.1.200 |

---

## Services Running

| Service | Type | Purpose | Status |
|---------|------|---------|--------|
| Pi-hole | LXC Container | Network-wide ad blocking and DNS filtering | ✅ Running |
| SOC Lab | VM | Attack simulation and detection with Splunk | 🔄 In Progress |
| Wazuh | LXC/VM | Self-hosted SIEM and security monitoring | 📅 Planned |
| Suricata | LXC/VM | Network intrusion detection system | 📅 Planned |
| Uptime Kuma | LXC Container | Service monitoring and alerting | 📅 Planned |
| Plex | LXC/VM | Personal media server | 📅 Planned |
| Nextcloud | LXC/VM | Self-hosted cloud storage | 📅 Planned |
| Tailscale | LXC Container | Secure remote access to homelab from anywhere | ✅ Running |
---

## Pi-hole — Network Wide Ad Blocker

[PiHole] <img width="1388" height="958" alt="PiHole" src="https://github.com/user-attachments/assets/d30b7b83-dcec-4dc7-b253-34f4da480041" />

### What is Pi-hole?
Pi-hole is a network-wide DNS sinkhole that blocks ads, trackers, and malicious domains at the DNS level before they ever reach your devices. Instead of blocking ads per browser or per device, Pi-hole protects every device on the network including phones, smart TVs, and game consoles.

### Deployment
- **Type:** LXC Container on Proxmox VE
- **Method:** Deployed using tteck's Proxmox Helper Scripts
- **DNS:** Router configured to point all DNS queries to Pi-hole

### Why LXC Instead of a Full VM?
LXC containers use significantly less RAM and CPU than full virtual machines while providing the same functionality for lightweight services like Pi-hole. This leaves more resources available for heavier workloads like Windows VMs and security tools.

### Blocklists Added
- Default Pi-hole blocklist
- Additional lists from firebog.net including:
  - Suspicious domains
  - Tracking and telemetry
  - Malware domains
  - Ad servers

### Results
Pi-hole is actively blocking DNS queries across all devices on the network, reducing ad traffic and blocking known malicious and tracking domains.
---

## Tailscale — Remote Access VPN

### What is Tailscale?
Tailscale creates a secure private network between all your devices using WireGuard under the hood. It allows remote access to your homelab from anywhere without complicated port forwarding or firewall rules.

### Deployment
- **Type:** Installed directly on Proxmox host
- **Method:** Official Tailscale install script
- **Authentication:** Connected to Tailscale account via CLI

### Configuration
- Proxmox accessible remotely via Tailscale IP at port 8006
- Pi-hole set as DNS nameserver for all Tailscale devices
- Override local DNS enabled — all Tailscale devices get network-wide ad blocking even away from home

### Benefits
- Access Proxmox UI from work or anywhere
- Pi-hole ad blocking works on MacBook even on work WiFi
- Secure encrypted tunnel — no open ports on home router

## SOC Analyst Lab *(In Progress)*

### Overview
A home Security Operations Center environment designed to simulate real enterprise attack and detection scenarios.

### Components
- **Kali Linux VM** — Attacker machine
- **Windows 10 VM** — Target machine with Sysmon installed
- **Splunk** — SIEM for log collection and analysis
- **Atomic Red Team** — Attack simulation framework
- **Sysmon** — Enhanced Windows event logging

### Lab Workflow
1. Sysmon generates detailed logs on the Windows VM
2. Logs are forwarded to Splunk via Universal Forwarder
3. Atomic Red Team simulates real attacker techniques on the Windows VM
4. Splunk detects and alerts on suspicious activity
5. Incidents are investigated and documented as reports

<img width="1384" height="957" alt="Screenshot 2026-05-26 220905" src="https://github.com/user-attachments/assets/74828e04-982d-470c-9fbf-025235546fdc" />


---

## Wazuh — Self-Hosted SIEM *(Planned)*

Wazuh is an open source security platform that provides threat detection, integrity monitoring, incident response, and compliance. Will be deployed as the primary SIEM once the SOC lab is complete.

---

## Suricata — Network IDS *(Planned)*

Suricata is an open source network threat detection engine that performs real-time intrusion detection, inline intrusion prevention, and network security monitoring. Will monitor all traffic on the homelab network.

---

## Uptime Kuma *(Planned)*

Self-hosted monitoring tool to track uptime and availability of all homelab services. Will send alerts when any service goes down.

---

## Network Diagram

Internet
│
Spectrum Router/Modem
│
├── Pi-hole LXC (DNS: 192.168.1.x)
├── Proxmox Host (192.168.1.200)
│       ├── Kali Linux VM
│       ├── Windows 10 VM
│       └── Splunk VM
└── Other devices (PC, MacBook, phones)

---

## Skills Demonstrated

- Virtualization and hypervisor management (Proxmox VE)
- Linux container (LXC) deployment and management
- Network DNS configuration and filtering
- Security operations center (SOC) environment design
- Virtual machine provisioning and configuration
- Network security monitoring
- Self-hosted service deployment and management

---

## Resources & Tools Used

- [Proxmox VE](https://www.proxmox.com)
- [Pi-hole](https://pi-hole.net)
- [tteck's Proxmox Helper Scripts](https://tteck.github.io/Proxmox)
- [Splunk Free](https://www.splunk.com)
- [Wazuh](https://wazuh.com)
- [Suricata](https://suricata.io)
- [Atomic Red Team](https://github.com/redcanaryco/atomic-red-team)
- [Firebog Blocklists](https://firebog.net)
