# Final Incident Investigation

## Objective

Correlate Windows Security and Sysmon telemetry from WS01 to reconstruct a controlled privileged-access incident.

## Controlled Activity

The investigation scenario uses the privileged account `WULAB\adm-amorgan` and includes:

- failed authentication attempts
- successful privileged authentication
- privileged command execution
- temporary addition of `WULAB\jlee` to the local `Administrators` group
- removal of `WULAB\jlee` from the local `Administrators` group

## Evidence Confirmed So Far

Windows Security telemetry in Splunk shows the relevant authentication and privileged-group event families, including:

- Event ID 4624 — successful logon
- Event ID 4672 — special privileges assigned
- Event ID 4732 — member added to a local security group
- Event ID 4733 — member removed from a local security group

Sysmon process-creation telemetry shows privileged command execution by `WULAB\adm-amorgan`, including `whoami.exe` and `net.exe` launched from PowerShell, with command lines involving the local `Administrators` group.

A final narrowed Splunk timeline will be captured to correlate the failed logon events (4625) with the successful privileged logon and group-change events in a single investigation window.

## ATT&CK Mapping

The controlled activity demonstrates monitoring concepts aligned with:

- **T1078 — Valid Accounts**: privileged account authentication and use
- **T1098 — Account Manipulation**: privileged group membership modification

The scenario is defensive and limited to the isolated lab environment.
