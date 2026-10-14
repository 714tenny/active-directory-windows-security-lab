# Active Directory / Windows Enterprise Security Lab

## Project Status

**Complete — Final Project Audit Passed**

This project builds and secures a small enterprise Windows environment centered on Active Directory Domain Services, Group Policy, least privilege, Windows security telemetry, Sysmon, Splunk, security assessment, remediation, and incident investigation.

The lab demonstrates the full defensive lifecycle:

**Identity Creation → Authentication → Authorization → Group Policy → Privileged Access → Security Logging → Detection → Investigation → Remediation → Validation**

## Environment

| System | Role | Lab Address |
|---|---|---|
| **DC01** | Active Directory Domain Services, DNS, Group Policy | `10.50.10.10` |
| **WS01** | Windows 11 Pro domain workstation | `10.50.10.20` |
| **SPLUNK01** | Splunk Enterprise monitoring and investigation | `10.50.10.30` |
| **KALI01** | Optional controlled testing host; not required for final build | `10.50.10.40` planned |

**Domain:** `corp.wulab.test`  
**NetBIOS:** `WULAB`  
**Network:** VMware VMnet2 host-only, `10.50.10.0/24`

## Key Security Outcomes

- Built a single-domain Active Directory environment with departmental OUs, users, and role-based security groups.
- Joined WS01 to the domain and validated domain authentication and DNS.
- Separated standard and privileged identities using `amorgan` and `adm-amorgan`.
- Enforced `GG-Workstation-Admins` membership in the local WS01 `Administrators` group through Group Policy Restricted Groups.
- Applied workstation account-lockout, interactive-logon, blank-password, and advanced audit-policy controls.
- Enabled Windows Security auditing for logon, special logon, process creation, user account management, and security group management.
- Installed Sysmon and validated endpoint telemetry on WS01.
- Forwarded Windows Security logs to Splunk index `windows` and Sysmon logs to index `sysmon`.
- Investigated authentication and privileged-access activity using Windows Event IDs `4624`, `4625`, `4672`, `4732`, and `4733`.
- Correlated Windows Security events with Sysmon Event ID `1` process telemetry and command-line evidence.
- Assessed Active Directory privilege and password settings, remediated identified weaknesses, and validated the final state.
- Hardened the default domain policy to a 14-character minimum password length, 24-password history, and 5-attempt / 15-minute lockout controls.
- Completed a PowerShell-based security audit of AD identities, privileged groups, password policy, local administrators, and Splunk Forwarder state.
- Reconstructed a controlled privileged-access incident and mapped the activity to MITRE ATT&CK concepts including **T1078 Valid Accounts** and **T1098 Account Manipulation**.

## Documentation

- [Lab Architecture](docs/architecture.md)
- [Security Scenario Investigations](docs/security-scenarios.md)
- [Active Directory Security Assessment](docs/ad-security-assessment.md)
- [PowerShell Security Audit](docs/powershell-security-audit.md)
- [Final Incident Investigation](docs/final-incident-investigation.md)
- [Final Project Audit](docs/final-project-audit.md)

Configuration and validation screenshots are stored under:

`images/active-directory/`

## Completed Milestones

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
- [x] Final project audit completed

## Skills Demonstrated

**Active Directory:** AD DS, DNS, OUs, users, groups, privileged identities, domain authentication  
**Windows Security:** Group Policy, least privilege, advanced auditing, local administrator control, account lockout  
**Detection & Monitoring:** Windows Event Logs, Sysmon, Splunk Universal Forwarder, SPL, event correlation  
**Security Operations:** assessment, remediation, validation, incident reconstruction, MITRE ATT&CK mapping  
**Automation & Administration:** PowerShell, Active Directory cmdlets, native Windows administration tools

## Project Safety

All activity was performed inside systems owned and controlled for this isolated lab. No public systems, real malware, or external credentials were targeted.
