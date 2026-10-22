Status: **Draft for review.** These are proposed hypotheses and measurement choices, not established results.

## Proposed testable hypotheses

| ID  | Hypothesis                                                                                                                                                  | Measurement                                                                                                          |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| H1  | Introducing the less-secure Linux endpoint increases externally reachable services relative to the hardened baseline.                                       | Reachable service count per endpoint and network total, measured from the same vantage point.                        |
| H2  | Under equivalent exposure and observation periods, tested legacy Windows versions differ in exposed services and observable suspicious connection attempts. | Services, connection attempts per endpoint-hour, and protocol distribution for each version.                         |
| H3  | Basic network security measures will reduce the exposure of legacy and unhardened endpoints, but will not eliminate all observable security risks.          | Comparison of reachable services, blocked connections, and remaining security exposures before and after mitigation. |

H2 tests differences among selected configurations; it cannot isolate OS age alone. A lack of observed hostile traffic is still a valid result. Reachability reductions do not by themselves demonstrate reduced compromise probability.

## Variables

- **Independent:** phase, endpoint security configuration, Windows version, mitigation condition.
- **Dependent:** externally reachable services, inter-host reachability, inbound attempts, firewall decisions, authentication failures, and evidence-supported hostile activity categories.
- **Controlled where possible:** VM resources, observation duration, capture location, scan source/settings, exposed addressing and NAT, workload, tool versions, and time synchronization.
- **Confounders to record:** host count, public IP changes, time of day, background traffic variation, service differences, logging differences, and packet loss.

| Variable type | What it means | Example from our project |
|---|---|---|
| **Independent** | What we intentionally change during the experiment. | Changing the Windows version, security configuration, or enabling mitigation measures. |
| **Dependent** | What we measure to see the effects of those changes. | Number of open ports, incoming connection attempts, failed logins, or blocked connections. |
| **Controlled** | What we try to keep the same to make the experiment fair. | Same network, observation duration, VM resources, and scanning procedures. |
| **Confounding** | Other factors that might influence our results without us intending them to. | Internet traffic changing throughout the day, differences in default Windows services, or packet loss. |

## Measurement plan

| Metric | Collection source | Reporting unit |
| --- | --- | --- |
| Reachable services | Approved scans and local service inventory | Per endpoint and network total; identify TCP/UDP and vantage point |
| Inbound connection attempts | Packet captures and firewall logs | Count and attempts per endpoint-hour; define counting rule |
| Authentication failures | Relevant authentication logs | Failures per endpoint-hour, with logging coverage noted |
| Firewall decisions | Firewall logs and counters | Allowed/blocked counts or rates, with rule configuration |
| Inter-host reachability | Controlled connectivity tests | Source/destination/protocol result matrix |
| Suspicious or confirmed hostile events | Correlated captures and logs | Categorized events linked to evidence |

## Decisions needed before implementation

- [x] Review or revise H1–H3.
- [ ] Choose exact OS versions and configuration differences.
- [ ] Define a connection attempt and event deduplication rules.
- [ ] Define criteria for suspicious activity and confirmed compromise.
- [ ] Choose equal observation windows and repeat count; record rationale.
- [ ] Decide whether phases retain previous hosts and how Windows versions replace one another.
- [ ] Decide whether repeated baseline/control windows are feasible to assess time variation.
- [ ] Specify external and internal measurement vantage points.
- [ ] Define scan timing so controlled test traffic can be separated from background traffic.
- [ ] Freeze tool settings, logging coverage, and procedures for each comparison.

Compare both per-endpoint and network totals. Record deviations rather than silently changing the procedure. See [[Analysis Plan]].
