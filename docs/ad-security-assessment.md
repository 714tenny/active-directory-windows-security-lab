# Active Directory Security Assessment

## Scope

The assessment reviews enabled identities, password-expiration exceptions, privileged group membership, and separation of standard and administrative identities in the `corp.wulab.test` lab domain.

## Evidence Reviewed

PowerShell queries were executed on DC01 using:

```powershell
Get-ADUser -Filter * -Properties Enabled,PasswordNeverExpires,PasswordLastSet |
  Select Name,SamAccountName,Enabled,PasswordNeverExpires,PasswordLastSet

Get-ADGroupMember "Domain Admins" |
  Select Name,SamAccountName,ObjectClass

Get-ADGroupMember "GG-Workstation-Admins" |
  Select Name,SamAccountName,ObjectClass
```

## Current Findings

### Finding 1 — Built-in Administrator password-expiration exception

The built-in domain `Administrator` account is enabled and currently has `PasswordNeverExpires = True`.

**Assessment:** Review/remediate in the lab because a permanently non-expiring privileged credential increases long-term credential risk.

### Finding 2 — Default Guest account disabled

The built-in `Guest` account is disabled.

**Assessment:** Secure configuration; no remediation required.

### Finding 3 — KRBTGT account state

The `krbtgt` account is disabled, which is expected for the built-in Kerberos service account.

**Assessment:** No remediation required based on the observed state.

### Finding 4 — Standard user password settings

Visible standard users including `amorgan`, `jlee`, `tdavis`, and `cbrooks` are enabled and show `PasswordNeverExpires = False`.

**Assessment:** Consistent with the lab's intended password-expiration model.

### Finding 5 — Domain Admins membership

The `Domain Admins` group contains only the built-in `Administrator` account in the observed output.

**Assessment:** No unnecessary user accounts were observed in the highest-privilege domain group.

### Finding 6 — Workstation administrator separation

The `GG-Workstation-Admins` group contains only the separate privileged identity `adm-amorgan`.

**Assessment:** Supports least privilege and separation between `amorgan` and `adm-amorgan`.

## Remaining Validation

Before closing the assessment, validate:

- the `adm-amorgan` password-expiration setting
- the effective default domain password policy

These checks will be used to finalize findings and perform remediation validation.
