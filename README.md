# Enterprise Network Redesign & Security Optimization — CIC Holdings PLC

An enterprise-grade network security redesign and virtualized simulation designed to eliminate flat network vulnerabilities, enforce departmental VLAN isolation, deploy Next-Generation Firewalling (NGFW), and establish digital forensic readiness[cite: 1].

---

## 📌 Project Overview
- **Author:** Enuka Geenad Meemanage[cite: 1]
- **Academic Context:** BSc (Hons) in Cyber Security and Digital Forensics, Kingston University London[cite: 1]
- **Target Organization:** CIC Holdings PLC[cite: 1]
- **Simulation Tools:** GNS3, Cisco Packet Tracer, VMware Workstation Pro, VMware Fusion[cite: 1]

### Problem Statement
The legacy network operated on a flat configuration with only two broadcast domains (one general LAN, one Wi-Fi), unrestricted lateral access between corporate departments, an exposed physical server room on the ground floor, and no link redundancy[cite: 1]. This left critical organizational data vulnerable to unauthorized internal movement, lateral attack escalation, and data exfiltration[cite: 1].

---

## 📥 Lab Topology Download
The complete virtualized network topology file is available via GitHub Releases:  
📦 **[Download GNS3 Project File (v1.0)](../../releases/tag/v1.0)**  
🔒 **SHA-256 Checksum:** `ebd40b1284a85fce65864ab90fa7c76acb0f9e5fcb94214238d54021bf1499c9`

---

## 🏗️ High-Level Architecture & Topology

![Architecture](<images/Light Final High Level Network Architecture Diagram.drawio.png>)

### Key Security Implementations
- **VLAN Segmentation:** Dedicated logical subnets across 8 departments, isolating sensitive data assets and restricting cross-departmental traffic[cite: 1].
- **Next-Generation Firewalling:** FortiGate firewall deployed for stateful inter-VLAN inspection, policy enforcement, and IDS/IPS threat protection[cite: 1].
- **Port Security Hardening:** Static and sticky MAC address filtering enabled on access-layer switches with violation protection to mitigate unauthorized hardware connectivity[cite: 1, 5, 6].
- **Enterprise Infrastructure Services:** Windows Server 2022 instances hosting Active Directory Domain Services (AD DS), DNS, DHCP, and secure FTP services[cite: 1].
- **Forensic Readiness & Centralized Logging:** Kiwi Syslog Server gathering Windows Security audit logs and network device telemetry via SolarWinds Event Log Forwarder[cite: 1].
- **High Availability (HA):** Redundant core switches and automated dual-ISP failover mechanisms to maintain business continuity[cite: 1].

---

## 📊 IP Addressing & VLAN Scheme
**Supernet:** `192.168.0.0/22` (Subnet Mask: `255.255.252.0`)[cite: 1]

| Floor | Department / Area | VLAN | Assigned Subnet Range | Host / Device Count |
| :--- | :--- | :--- | :--- | :--- |
| **Ground** | Conference Room (Staff Wi-Fi) | VLAN 11 | `192.168.0.81 - 192.168.0.82` | 2 Access Points[cite: 1] |
| **Ground** | Reception (Guest Wi-Fi) | VLAN 12 | `192.168.0.83 - 192.168.0.84` | 2 Access Points[cite: 1] |
| **Ground** | Health Care | VLAN 13 | `192.168.0.130 - 192.168.0.227` | 100 Workstations[cite: 1] |
| **1st** | Agriculture (Agri) | VLAN 21 | `192.168.0.230 - 192.168.0.307` | 80 Workstations[cite: 1] |
| **1st** | Human Resources (HR) | VLAN 22 | `192.168.0.310 - 192.168.0.327` | 20 Workstations[cite: 1] |
| **1st** | Shipping | VLAN 23 | `192.168.0.330 - 192.168.0.347` | 20 Workstations[cite: 1] |
| **2nd** | Industrial Chemicals | VLAN 31 | `192.168.0.350 - 192.168.0.387` | 40 Workstations[cite: 1] |
| **2nd** | IT Department | VLAN 32 | `192.168.0.390 - 192.168.0.417` | 30 Workstations[cite: 1] |
| **2nd** | Secure Server Farm | VLAN 101 | `192.168.0.65 - 192.168.0.78` | 5 Servers[cite: 1] |
| **3rd** | Cooperation | VLAN 41 | `192.168.0.510 - 192.168.0.523` | 16 Workstations[cite: 1] |
| **3rd** | Finance | VLAN 42 | `192.168.0.420 - 192.168.0.507` | 90 Workstations[cite: 1] |

---

## ⚙️ Configuration Snippets

### 1. Cisco L2 Switch Port Security
```cisco
interface range Ethernet0/1 - 2
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation protect
 exit
```

### 2. FortiGate Firewall Interface Setup (CLI)
```fortios
config system interface
    edit "port"
        set mode static
        set ip 192.168.253.135/24
        set alias "WAN"
        set role wan
        set allowaccess ping https ssh http fgfm
    next
    edit "port2"
        set mode static
        set ip 192.168.0.2/24
        set alias "LAN"
        set role lan
        set allowaccess ping https ssh http
    next
end
```

---

## 🧪 Implementation Verification & Evidence

### 1. Switch VLAN Segmentation & Interface Assignment
VLAN creation and interface mappings verified on access layer switches:
![VLAN Creation](<images/VLAN Creation Third Floor.png>)  
![VLAN Interface Assignment](<images/VLAN Interface Assignment.png>)

### 2. Switch Layer 2 Port Security Verification
Switch access ports configured with sticky MAC learning and violation protect rules to prevent unauthorized device access:
![Port Security Setup](<images/Cisco Port Security Setup.png>)  
![Port Security Verification](<images/Port Security Verification.png>)

### 3. FortiGate NGFW Interface & Web Management
Initial KVM virtual machine setup, CLI interface addressing, and management dashboard access:
![FortiGate VM Initial Boot](<images/FortiGate VM Initial Boot.png>)  
![FortiGate Interface IP Config](<images/FortiGate Interface IP Config.png>)  
![FortiGate Web Login](<images/FortiGate Web Login and CLI.png>)  
![FortiGate Dashboard](<images/FortiGate Dashboard Overview.png>)

### 4. Enterprise Core Services (DNS, DHCP, FTP)
Automated client IP assignment, domain resolution for `corp.cic-dev.com`, and departmental file share verification:
![DHCP Verification](<images/Working DHCP.png>)  
![DHCP Settings](<images/Working DHCP 01.png>)  
![DNS Service](<images/Working DNS 01.png>)  
![FTP Shared Folder 01](<images/Working FTP 01.png>)  
![FTP Shared Folder 02](<images/Working FTP 02.png>)  
![FTP Shared Folder 03](<images/Working FTP 03.png>)

### 5. Centralized Syslog Ingestion & Forensic Auditing
Real-time Windows Security auditing events (Logon ID `4624` / Logoff ID `4634`) and network telemetry indexed into Kiwi Syslog Server:
![Syslog Setup](<images/Working Syslog 01.png>)  
![Syslog Log Capture](<images/Working Syslog 02.png>)

---

## 📁 Repository Structure
```text
├── docs/              # Assessment project final report PDF
├── images/            # Floor plans, topologies, and test captures
└── README.md          # Project documentation and architecture details
```
