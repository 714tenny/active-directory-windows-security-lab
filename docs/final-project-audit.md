# Final Project Audit

## Audit Result

**PASS — Project Complete**

The final audit reviewed repository structure, documentation consistency, security-control status, investigation evidence, and recruiter-facing readability.

## Repository Review

Verified final structure:

- `README.md`
- `docs/architecture.md`
- `docs/security-scenarios.md`
- `docs/ad-security-assessment.md`
- `docs/powershell-security-audit.md`
- `docs/final-incident-investigation.md`
- `docs/final-project-audit.md`
- `images/active-directory/`

The README now acts as the project landing page and links directly to each detailed phase document.

## Documentation Checks

- Removed stale "next step" and "currently configuring" language from completed phases.
- Updated architecture tables from planned/reserved states to the final validated state.
- Removed references to evidence images that were not present in the repository so the documentation does not contain broken image links.
- Corrected the PowerShell multiline syntax shown in the AD remediation documentation.
- Confirmed the project checklist reflects completion of all major milestones.
- Added a concise summary of security outcomes, event IDs, Splunk indexes, hardening results, and MITRE ATT&CK mapping to the README.

## Security-Control Review

Final documented controls include:

- Active Directory Domain Services and DNS
- Departmental OUs and security groups
- Separate standard and privileged identities
- Group Policy Restricted Groups for workstation administration
- Workstation account-lockout and interactive-logon controls
- Advanced Windows auditing
- Sysmon endpoint telemetry
- Splunk Universal Forwarder and centralized log collection
- 14-character domain minimum password length
- 24-password history
- 5-attempt lockout threshold
- 15-minute lockout duration and observation window
- PowerShell-based validation of privileged groups and service state

## Detection and Investigation Review

Documented investigations cover:

- failed and successful domain authentication
- privileged authentication and Event ID 4672
- local Administrators group additions and removals
- Sysmon process creation and command-line context
- final correlation of Windows Security and Sysmon telemetry

Relevant Windows event IDs include:

- `4624` — successful logon
- `4625` — failed logon
- `4672` — special privileges assigned
- `4732` — member added to local security group
- `4733` — member removed from local security group

The final investigation also maps controlled activity to **T1078 Valid Accounts** and **T1098 Account Manipulation**.

## Scope Notes

This is an intentionally small, isolated home lab rather than a production enterprise deployment. KALI01 remained optional and was not required to demonstrate the defensive objectives. The final portfolio emphasizes Windows identity security, least privilege, endpoint telemetry, Splunk monitoring, assessment, remediation, and incident investigation.

## Final Assessment

The repository is internally consistent, documents the complete security lifecycle, avoids unsupported claims, and is suitable to present as a hands-on Windows / Active Directory security portfolio project.
