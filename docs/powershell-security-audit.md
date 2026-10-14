# PowerShell Security Audit

## Objective

Validate key Active Directory, privileged-access, workstation-administrator, password-policy, and Splunk Forwarder security settings using PowerShell and native Windows command-line tools.

## DC01 Validation

The following checks were executed on DC01:

```powershell
Get-ADUser -Filter * -Properties Enabled,PasswordNeverExpires |
  Select Name,SamAccountName,Enabled,PasswordNeverExpires

Get-ADGroupMember "Domain Admins" |
  Select Name,SamAccountName,ObjectClass

Get-ADGroupMember "GG-Workstation-Admins" |
  Select Name,SamAccountName,ObjectClass

Get-ADDefaultDomainPasswordPolicy |
  Select MinPasswordLength,PasswordHistoryCount,MaxPasswordAge,LockoutThreshold,LockoutDuration
```

### Observed Results

- Built-in `Administrator` is enabled with `PasswordNeverExpires = False`.
- `Guest` is disabled.
- Standard lab users do not have password-expiration exemptions.
- `Domain Admins` contains only the built-in `Administrator` account.
- `GG-Workstation-Admins` contains only the separate privileged identity `adm-amorgan`.
- Minimum password length is `14`.
- Password history count is `24`.
- Maximum password age is `42 days`.
- Account lockout threshold is `5`.
- Account lockout duration is `15 minutes`.

## WS01 Validation

The following checks were executed on WS01:

```powershell
net localgroup administrators
sc.exe qc SplunkForwarder
sc.exe query SplunkForwarder
```

### Observed Results

Local Administrators membership included:

- `Administrator`
- `labadmin`
- `WULAB\Domain Admins`
- `WULAB\GG-Workstation-Admins`

Splunk Forwarder service validation confirmed:

- Service name: `SplunkForwarder`
- Start type: `AUTO_START`
- Service account: `LocalSystem`
- State: `RUNNING`

## Audit Outcome

The PowerShell security audit confirmed that the intended least-privilege model, privileged identity separation, hardened domain password policy, workstation administrator enforcement, and Splunk Forwarder service configuration remain in place and operational.
