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
