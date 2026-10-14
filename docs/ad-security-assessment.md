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


## Remediation and Validation

The assessment identified three remediation targets:

1. The built-in domain `Administrator` account had `PasswordNeverExpires = True`.
2. The default domain minimum password length was `7`.
3. The default domain account lockout threshold was `0`, meaning domain accounts were not locked out after repeated failed attempts.

The following remediations were applied on DC01:

```powershell
Set-ADUser Administrator -PasswordNeverExpires $false

Set-ADDefaultDomainPasswordPolicy -Identity "corp.wulab.test" \
  -MinPasswordLength 14 \
  -LockoutThreshold 5 \
  -LockoutDuration (New-TimeSpan -Minutes 15) \
  -LockoutObservationWindow (New-TimeSpan -Minutes 15)
```

Validation confirmed:

- Built-in `Administrator`: `PasswordNeverExpires = False`
- Minimum password length: `14`
- Password history count: `24`
- Maximum password age: `42 days`
- Account lockout threshold: `5`
- Account lockout duration: `15 minutes`
- Account lockout observation window: `15 minutes`

The separate privileged workstation administrator account `adm-amorgan` was also validated as enabled with `PasswordNeverExpires = False`.

### Assessment Outcome

The observed privileged-group memberships were appropriately constrained, standard users did not have password-expiration exemptions, and the identified password/lockout weaknesses were remediated and verified. The AD security assessment and remediation-validation phase is complete.
