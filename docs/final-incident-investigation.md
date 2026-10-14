# Final Incident Investigation

## Objective

Correlate Windows Security and Sysmon telemetry from WS01 to reconstruct a controlled privileged-access incident.

## Controlled Activity

The final investigation focused on the privileged account `WULAB\adm-amorgan` and a temporary change to local administrative access on WS01.

The activity included:

- privileged authentication and administrative use
- execution of `whoami.exe` and `net.exe` from PowerShell
- temporary addition of `WULAB\jlee` to the local `Administrators` group
- removal of `WULAB\jlee` from the local `Administrators` group

Failed-logon monitoring had already been validated separately in Scenario 1 with Event ID 4625. The final narrowed incident timeline did not show a matching 4625 event for `adm-amorgan`, so the investigation does not claim one for this specific sequence.

## Windows Security Evidence

Splunk searches confirmed relevant privileged-access and group-change events on `WS01`, including:

- Event ID 4624 — successful logon
- Event ID 4672 — special privileges assigned
- Event ID 4732 — member added to a local security group
- Event ID 4733 — member removed from a local security group

A narrowed timeline for `adm-amorgan` showed the local `Administrators` group modifications performed by the privileged account.

## Sysmon Process Evidence

Sysmon Event ID 1 process-creation telemetry showed:

- user: `WULAB\adm-amorgan`
- `whoami.exe` launched from PowerShell
- `net.exe` launched from PowerShell
- command lines for local `Administrators` group enumeration and controlled `WULAB\jlee` add/remove operations

This provides process-level evidence that correlates the administrative commands with the Windows Security group-change events.

## Investigation Conclusion

The combined Windows Security and Sysmon telemetry was sufficient to reconstruct the privileged administrative activity on WS01. Windows Security identified the security-relevant group changes, while Sysmon provided the process and command-line context that explained how those changes were performed.

The temporary administrative membership was removed during the controlled test, returning WS01 to the intended least-privilege state.

## MITRE ATT&CK Mapping

The controlled activity demonstrates monitoring concepts aligned with:

- **T1078 — Valid Accounts**: use of a valid privileged account
- **T1098 — Account Manipulation**: modification of privileged group membership

All activity was performed inside the isolated lab environment.
