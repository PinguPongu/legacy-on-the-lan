## Experimental Variables

### Independent Variables

Variables deliberately changed between experimental conditions:

- **Endpoint security configuration:** Hardened versus less-hardened configurations.
- **Operating-system version:** Different legacy Windows versions tested sequentially.
- **Mitigation condition:** Before and after implementing selected defensive controls.
- **Experimental phase:** Used to identify the tested combination of endpoints and configurations, rather than treated as an independent causal factor.

### Dependent Variables

Outcomes measured during the experiments:

- Number of externally reachable services.
- Number and rate of inbound connection attempts.
- Number of unique external source IP addresses.
- Authentication failures.
- Firewall allow/block events.
- Inter-host service reachability.
- Number and type of evidence-supported suspicious events.

### Controlled Variables

Conditions kept consistent wherever technically feasible:

- Network topology and isolation.    
- Endpoint placement and external exposure policy.
- NAT and port-forwarding configuration.
- Monitoring and scanning vantage points.
- Observation duration and procedures.
- VM resource allocation.
- Monitoring tool versions and settings.
- Time synchronization.
- Background workloads and other active endpoints.

**Phase 3:** Each legacy Windows version will be tested separately in the same isolated network position, with only one Windows test endpoint active at a time.

### Confounding Variables

Factors that may affect results independently of the intended changes:

- Natural variation in Internet activity.
- Time of day and day of week.
- Different default services and protocols between OS versions.
- Patch levels and operating-system configuration differences.
- Changes in the public-facing IP address.
- Incomplete logs or dropped capture packets.
- Differences in host count between experimental phases.
- Differences in service availability or uptime.

Potential confounding factors will be documented and considered during analysis.

---

## Metrics and Measurement Definitions

|Metric|Operational definition|Reporting unit|
|---|---|---|
|Reachable services|Unique reachable protocol/port combinations from a specified vantage point|Count per endpoint and network|
|Inbound TCP attempts|Initial TCP SYN attempts, excluding identified retransmissions and known test traffic|Attempts per valid observation hour|
|Unique source IPs|Distinct external source addresses observed during the measurement window|Count per window|
|Authentication failures|Failed authentication events identified in available system logs|Failures per valid observation hour|
|Firewall blocks|Logged firewall block decisions, with packet- or connection-level semantics documented|Count/rate|
|Inter-host reachability|Results of controlled source-to-destination connectivity tests|Reachable / blocked / inconclusive|
|Suspicious events|Deduplicated events matching predefined suspicious-activity indicators|Events per window|

### General Counting Rules

1. Record measurement start and end times in UTC.
2. Record actual valid observation duration.
3. Exclude known controlled scanning and testing traffic from unsolicited-activity counts.
4. Deduplicate TCP retransmissions using a documented method.
5. Avoid double-counting the same event across packet captures and system logs.
6. Count TCP and UDP observations separately.
7. Document logging gaps, dropped packets, and incomplete observations.
8. Preserve raw evidence and reference it from experiment notes.
9. Report endpoint-level measurements and network totals separately.
10. Apply the same counting rules across comparable experimental runs.

---

## Event Classification

|Category|Description|
|---|---|
|Routine / Unclassified|Activity without sufficient evidence of malicious intent|
|Reconnaissance-like|Patterns consistent with service discovery or scanning|
|Authentication-related|Failed or suspicious authentication activity|
|Exploit-like|Activity resembling attempts to exploit a service or vulnerability|
|Confirmed Security Incident|Unauthorized access or execution supported by corroborating evidence|
|Unknown|Activity that cannot be reliably classified using available evidence|

### Classification Principles

- Open ports do not automatically imply exploitable vulnerabilities.
- Suspicious traffic does not automatically establish malicious intent.
- A failed login is not necessarily an attack.
- Confirmed compromise requires stronger evidence than network activity alone.
- Classification rules should be finalized before official data collection.
- Uncertain events should remain unclassified rather than be forced into a category.

---

## Methodological Notes

The legacy Windows systems will be evaluated sequentially under equivalent network conditions.

Differences between Windows versions will be interpreted as differences between tested operating-system configurations, rather than proof that operating-system age alone caused the observed effects.

The observation of hostile activity is not guaranteed. An absence of detected activity is a valid experimental result and will be reported accordingly.

Measurement definitions and event classification criteria may be refined during pilot testing. Any subsequent changes will be documented and justified.