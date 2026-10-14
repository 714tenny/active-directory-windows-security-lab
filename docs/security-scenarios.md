# Security Scenario Investigations

## Scenario 1 — Failed and Successful Domain Logons

### Objective

Validate that WS01 authentication activity is captured by Windows Security auditing, forwarded to Splunk, and can be investigated with SPL.

### Controlled Test

On WS01, the domain account `WULAB\amorgan` was used for three intentional failed sign-in attempts, followed by a successful sign-in.

### Splunk Validation

Failed logons were identified with:

```spl
index=windows host=WS01 EventCode=4625 earliest=-15m
```

The failed authentication event showed the target user `amorgan` in the `WULAB` domain. The event recorded status `0xc000006d` with substatus `0xc000006a`, consistent with an invalid credential attempt.

Successful and failed authentication activity was summarized with:

```spl
index=windows host=WS01 (EventCode=4624 OR EventCode=4625) earliest=-15m
| stats count by EventCode
```

Observed results:

- Event ID 4624 — 6 successful logon events
- Event ID 4625 — 3 failed logon events

### Result

The test confirmed that domain authentication activity from WS01 is being audited and forwarded to Splunk successfully. This provides the telemetry required for authentication monitoring and failed-logon investigation.


## Scenario 2 — Privileged Domain Logon

### Objective

Validate that use of the separate privileged identity `WULAB\adm-amorgan` is visible in centralized Windows Security telemetry.

### Controlled Test

The privileged account `WULAB\adm-amorgan` was used to sign in to WS01.

### Splunk Validation

Successful logon activity was confirmed with:

```spl
index=windows host=WS01 EventCode=4624 TargetUserName="adm-amorgan" earliest=-2h
| table _time TargetUserName TargetDomainName LogonType IpAddress
```

Observed results included two Event ID 4624 records for `adm-amorgan` in the `WULAB` domain. The events reported Logon Type 11 and loopback address `::1`.

Special-privilege assignment was confirmed with:

```spl
index=windows host=WS01 EventCode=4672 SubjectUserName="adm-amorgan" earliest=-2h
| table _time SubjectUserName SubjectDomainName
```

Observed result:

- Event ID 4672 for `adm-amorgan`
- Subject domain: `WULAB`

### Result

The test confirmed that privileged account usage on WS01 is visible in Splunk through both successful logon telemetry (4624) and special-privilege assignment telemetry (4672). This supports monitoring of privileged access and separation of standard and administrative identities.
