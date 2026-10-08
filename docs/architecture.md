# Lab Architecture

## Status

**Architecture Planned — Deployment Not Started**

## Purpose

The Active Directory / Windows Enterprise Security Lab simulates a small enterprise Windows environment for practicing identity administration, authentication security, Group Policy, least privilege, Windows security monitoring, detection, investigation, and remediation.

The environment is intentionally small enough to operate as a home lab while maintaining realistic relationships between enterprise Windows systems.

---

## Active Directory Environment

### Forest / Domain

**DNS Domain:** `corp.wulab.test`

**NetBIOS Domain:** `WULAB`

The lab uses a single forest and a single Active Directory domain.

This design is appropriate for a small enterprise lab because it provides the core Active Directory identity and authentication experience without adding unnecessary multi-domain or multi-forest complexity.

---

## Systems

| Hostname | Operating System | Role | Planned IP Address |
|---|---|---|---|
| DC01 | Windows Server | Active Directory Domain Services, DNS, Group Policy, Authentication | 10.50.10.10 |
| WS01 | Windows 11 | Domain Workstation / Security Testing Endpoint | 10.50.10.20 |
| SPLUNK01 | Existing Splunk Environment | Security Monitoring and Investigation | 10.50.10.30 |
| KALI01 | Kali Linux | Optional Controlled Security Testing | 10.50.10.40 |

---

## Virtual Network

**Platform:** VMware Workstation

**Planned Virtual Network:** VMnet2

**Network Type:** NAT

**Subnet:** `10.50.10.0/24`

**Subnet Mask:** `255.255.255.0`

**Planned NAT Gateway:** `10.50.10.2`

**VMware DHCP:** Disabled

VMnet2 will provide an isolated private network for the Active Directory lab while allowing controlled outbound connectivity through VMware NAT.

Infrastructure systems will use manually assigned IP addresses. No inbound NAT port forwarding will be configured.

### Planned Addressing

| Device | IP Address | Status |
|---|---|---|
| DC01 | 10.50.10.10 | Reserved |
| WS01 | 10.50.10.20 | Reserved |
| SPLUNK01 | 10.50.10.30 | Reserved |
| KALI01 | 10.50.10.40 | Reserved |

---

## DNS Design

DC01 will host DNS for the Active Directory domain.

Domain-joined workstations will use:

`10.50.10.10`

as their primary DNS server.

Active Directory relies heavily on DNS to locate Domain Controllers and domain services. Proper DNS configuration is therefore required for domain joining, authentication, Group Policy, and general Active Directory functionality.

---

## Virtual Machine Resources

### DC01

- 2 vCPU
- 4 GB RAM
- 70 GB thin-provisioned storage

### WS01

- 2 vCPU
- 6 GB RAM
- 80 GB thin-provisioned storage

### SPLUNK01

- 2–4 vCPU
- Approximately 6 GB RAM
- Existing Splunk environment

### KALI01

Optional.

- 2 vCPU
- 4 GB RAM
- 40 GB storage

KALI01 will normally remain powered off unless required for a controlled lab exercise.

---

## Organizational Unit Design

Planned OU structure:

```text
CORP
├── Users
│   ├── IT
│   ├── Security
│   ├── Finance
│   └── HR
├── Computers
│   ├── Workstations
│   └── Servers
├── Groups
└── Service Accounts
