# Active Directory / Windows Enterprise Security Lab

## Project Status

**Phase 14 — Final Project Audit**

DC01 and WS01 are fully integrated with `corp.wulab.test`. Windows Security and Sysmon telemetry from WS01 are now being forwarded to Splunk on SPLUNK01 and validated in dedicated indexes. The final incident investigation is complete. Windows Security and Sysmon telemetry were correlated to reconstruct privileged command execution and local Administrators group changes on WS01. The remaining phase is the final project and GitHub audit.

## Overview

This project builds a small enterprise Windows environment centered on Active Directory Domain Services and Windows identity security.

The lab is designed to demonstrate practical experience with:

- Active Directory Domain Services
- Windows Server
- DNS
- Organizational Units
- Users and Security Groups
- Windows domain authentication
- Kerberos and NTLM
- Group Policy
- Least privilege
- Windows Security auditing
- Sysmon
- PowerShell
- Splunk
- Authentication monitoring
- Privileged-access monitoring
- Detection engineering
- Security assessment
- Remediation validation
- Incident investigation
- MITRE ATT&CK

## Project Objective

The objective is to demonstrate the enterprise Windows identity and security lifecycle:

**Identity Creation → Authentication → Authorization → Group Policy → Privileged Access → Security Logging → Detection → Investigation → Remediation → Validation**

## Planned Architecture

The lab will contain:

- **DC01** — Windows Server Domain Controller
- **WS01** — Windows 11 domain workstation
- **SPLUNK01** — Splunk SIEM
- **KALI01** — Optional controlled testing workstation

### Active Directory Domain

`corp.wulab.test`

### Lab Network

`10.50.10.0/24`

Detailed architecture documentation is located in:

`docs/architecture.md`

## Current Progress

- [x] Project scope defined
- [x] Initial repository structure created
- [x] Architecture planned
- [x] VMware network configured
- [x] Windows Server deployed
- [x] Active Directory Domain Services configured
- [x] Organizational Units configured
- [x] Users and security groups configured
- [x] Windows workstation domain joined
- [x] Authentication validated
- [x] Group Policy security controls implemented
- [x] Least-privilege model implemented
- [x] Windows security auditing configured
- [x] Sysmon deployed
- [x] Splunk log forwarding configured
- [x] Security scenarios investigated
- [x] Active Directory security assessment completed
- [x] Findings remediated and validated
- [x] PowerShell security audit completed
- [x] Final incident investigation completed
- [ ] Final project audit completed

## Security Focus

This project focuses on traditional enterprise Windows security, including:

- Identity administration
- Domain authentication
- Authorization
- Group-based access
- Privileged access
- Windows endpoint security
- Group Policy
- Security logging
- Authentication monitoring
- Identity-change detection
- Investigation
- Remediation
- Validation

## Project Safety

All testing is performed inside systems owned and controlled for this lab.

The project does not involve:

- Attacking public systems
- Real malware
- Publicly exposed vulnerable services
- Credential attacks against external systems
