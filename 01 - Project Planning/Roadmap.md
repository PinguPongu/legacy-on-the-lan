# Legacy on the LAN — Project Timeline

**Project:** Legacy on the LAN  
**Start Date:** 15 October 2026  
**Presentation Date:** 6 November 2026  
**Time Available:** 22 days until presentation  
**Status:** Preparation

## Project Overview

The project is divided into five stages:

1. Research design and preparation.
2. Lab setup and pilot testing.
3. Main experiments (Phases 1–3).
4. Mitigation testing and analysis (Phase 4).
5. Report writing and presentation preparation.    

The objective is to complete experimental data collection by **31 October**, leaving approximately five days for analysis, presentation preparation, and final adjustments.

---

## Week 1 — Preparation and Lab Setup

**15–21 October**

### 15–18 October — Research Design and Preparation

- Finalize research hypotheses (H1, H2, H3).
- Identify independent, dependent, controlled, and confounding variables.
- Define metrics, counting rules, and event categories.
- Decide observation durations and repeat counts.
- Select the Linux and Windows versions to test.
- Finalize the experimental methodology.
- Prepare documentation templates in Obsidian.
- Confirm lab requirements and authorization with Frostbyte.

**Milestone — 18 October:** Research design and measurement protocol completed.

### 19–21 October — Lab Setup and Pilot Testing

- Configure the isolated experimental network.
- Verify isolation from unrelated systems.
- Confirm authorized Internet exposure and routing.
- Create the necessary virtual machines.
- Configure Nmap, tcpdump/Wireshark, and logging.
- Verify timestamps and data collection.
- Perform controlled connectivity tests.
- Run a short pilot experiment.
- Document and resolve technical issues.

**Milestone — 21 October:** Lab environment operational and ready for official experiments.

---

## Week 2 — Main Experiments

**22–28 October**

### 22–23 October — Phase 1: Hardened Linux Baseline

- Deploy hardened Linux.
- Verify firewall rules and enabled services.
- Begin network monitoring.
- Perform standardized service discovery.
- Record incoming connection attempts.
- Collect firewall and system logs.
- Preserve baseline evidence.

**Milestone — 23 October:** Baseline measurements completed.

### 24–25 October — Phase 2: Less-Hardened Linux

- Introduce the less-hardened Linux endpoint.
- Document configuration differences.
- Repeat baseline measurements.
- Capture network activity.
- Collect relevant logs.
- Compare service exposure against Phase 1.
- Document preliminary observations.

**Milestone — 25 October:** Phase 2 measurements completed.

### 26–28 October — Phase 3: Legacy Windows Testing

Each Windows version will be tested **individually in the same isolated network environment**.

- Deploy the first Windows version.
- Perform standardized scans and monitoring.
- Export and preserve results.
- Restore the standardized test environment.
- Repeat with the next Windows version.
- Maintain consistent network exposure and observation procedures.
- Document differences in services and configurations.
- Compare results between Windows versions.

**Milestone — 28 October:** Legacy Windows comparisons completed.

> [!important]  
> The number of Windows versions tested will depend on the available observation time. Prioritize reliable, comparable results over testing too many systems.

---

## Week 3 — Mitigation, Analysis, and Presentation

**29 October – 6 November**

### 29–30 October — Phase 4: Mitigation Testing

- Select the security controls to evaluate.
- Apply firewall restrictions and/or network isolation.
- Repeat relevant exposure measurements.
- Perform before/after reachability tests.
- Measure changes in accessible services.
- Identify remaining exposed services or risks.
- Document mitigation effectiveness and limitations.

**Milestone — 30 October:** Mitigation testing completed.

### 31 October – 2 November — Results and Analysis

- Organize evidence from all experimental phases.
- Verify data completeness.
- Calculate relevant measurement rates.
- Compare service exposure across configurations.
- Compare observed network activity.
- Evaluate hypotheses H1, H2, and H3.
- Create graphs and comparison tables.
- Identify limitations and confounding factors.
- Write preliminary conclusions.

**Milestone — 2 November:** Results and conclusions drafted.

### 3–5 November — Presentation Preparation

- Prepare presentation structure.
- Explain the research question and objectives.
- Describe the experimental methodology.
- Present the most important findings.
- Include relevant graphs and evidence.
- Discuss limitations and unexpected results.
- Explain conclusions and future research possibilities.
- Rehearse the presentation.
- Make final improvements.

**Milestone — 5 November:** Presentation ready.

### 6 November — Project Presentation

- Present Legacy on the LAN.
- Explain the methodology and findings.
- Discuss whether the hypotheses were supported.
- Answer questions about experimental design and limitations.

---

## Key Deadlines

|Date|Milestone|
|---|---|
|18 October|Research design finalized|
|21 October|Lab setup and pilot completed|
|23 October|Phase 1 completed|
|25 October|Phase 2 completed|
|28 October|Phase 3 completed|
|30 October|Phase 4 completed|
|31 October|Official data collection finished|
|2 November|Analysis and conclusions drafted|
|4 November|Presentation slides completed|
|5 November|Final rehearsal|
|**6 November**|**Project presentation**|

---

## Project Priorities

### Priority 1 — Reliable Experimental Data

A smaller number of well-documented experiments is preferable to many incomplete or inconsistent tests.

### Priority 2 — Comparable Windows Testing

Each legacy Windows version should be tested separately under equivalent conditions.

### Priority 3 — Evidence-Based Conclusions

Distinguish exposed services, suspicious activity, potential vulnerabilities, and confirmed security incidents.

### Priority 4 — Protect Time for Analysis

Avoid extending experimental work into the final presentation preparation period unless necessary.

### Priority 5 — Maintain Documentation

Record configurations, observations, decisions, and limitations throughout the project rather than reconstructing them afterward.

---

## Contingency Plan

If the lab setup or experiments take longer than expected:

1. Reduce the number of Windows versions tested.
2. Prioritize the hardened baseline and at least one meaningful comparison.
3. Keep the mitigation evaluation focused on clearly measurable controls.
4. Document any shortened observation periods.
5. Reserve sufficient time to analyze and present the results.

**Final deadline: 6 November 2026**




---

# Phase Configuration Plan

**Project:** Legacy on the LAN  
**Status:** Draft — Pending Review  
**Purpose:** Define the configuration of each experimental phase and establish how changes will be documented.

## 1. Objective

Ensure that all experimental phases are documented consistently, allowing differences in network exposure and observed activity to be compared.

The experiment will consist of four phases, with each phase introducing a specific change to the experimental environment.

---

## 2. Experimental Phase Overview

|Phase|Configuration|What Changes?|What Is Compared?|
|---|---|---|---|
|**1 — Baseline**|Hardened Ubuntu 26.04.1 LTS with updates, firewall, Fail2ban, minimal services, and logging|Establish initial configuration|Baseline exposure and network activity|
|**2 — Less-Secure Linux**|Add Linux Mint 22.3 with documented weaker security settings|Additional endpoint and security configuration|Network-wide and per-endpoint exposure|
|**3 — Legacy Windows**|Test selected Windows versions individually in the same isolated test position|Windows version changes between runs|Reachable services, protocols, and observed activity|
|**4 — Mitigation**|Apply selected firewall restrictions, network isolation, or other defensive controls|Additional security measures|Before/after exposure and reachability|

---

## 3. Phase 1 — Hardened Linux Baseline

### Configuration

**Operating System:** Ubuntu 26.04.1 LTS  
**Role:** Hardened baseline endpoint

Planned security configuration:

- Install available security updates.
- Enable and configure the host firewall.
- Install and configure Fail2ban.
- Minimize unnecessary network services.
- Enable relevant system and security logging.
- Document network interfaces, IP addresses, and exposed services.

### Measurements

- Externally reachable TCP/UDP services.
- Incoming connection attempts.
- Unique external source IP addresses.
- Authentication failures.
- Firewall block events.
- Relevant suspicious activity.

### Documentation

- Record installed OS version and updates.
- Record firewall rules.
- Record enabled and listening services.
- Document IP configuration.
- Record VM resources.
- Create and verify baseline snapshot.
- Record monitoring configuration.

**Expected output:** A documented baseline against which later phases can be compared.

---

## 4. Phase 2 — Less-Secure Linux

### Configuration

**Operating System:** Linux Mint 22.3  
**Role:** Less-hardened experimental endpoint

Introduce the endpoint into the authorized experimental network.

Proposed configuration differences:

- Less restrictive firewall configuration.
- Additional network services compared with the hardened baseline.
- Reduced hardening measures.
- Clearly documented security configuration.

Specific settings and exposed services must be selected and approved before implementation.

### Measurements

Repeat the Phase 1 measurements using consistent procedures.

Record both individual endpoint results and network-wide totals.

### Documentation

- Record Linux Mint version and patch level.
- Document security configuration differences.
- Record enabled network services.
- Document firewall settings.
- Record VM resources and network configuration.
- Create and verify snapshot.
- Record changes relative to Phase 1.

**Expected output:** A comparison of network exposure before and after introducing the less-hardened endpoint.

**Important:** This phase changes both endpoint count and security configuration. These effects cannot automatically be separated.

---

## 5. Phase 3 — Legacy Windows Testing

### Configuration

**Operating Systems:** Selected legacy Windows versions (TBD)

Each Windows version will be tested **individually in the same isolated network test position**.

Only one legacy Windows test endpoint will be active at a time.

### Conditions to Keep Consistent

- Network topology and isolation.
- Internet exposure and upstream firewall rules.
- NAT and port-forwarding configuration.
- IP addressing or equivalent network placement.
- Monitoring and scanning procedures.
- Observation duration.
- VM resources where technically feasible.
- Number and configuration of other active endpoints.   

### Measurements

For each Windows version:

- Externally reachable services.
- Exposed protocols and service versions.
- Incoming connection attempts.
- Authentication failures where logging supports them.
- Firewall activity.
- Suspicious activity supported by collected evidence.

### Documentation

- Record Windows version, edition, and build.
- Record patch level.
- Document enabled network services.
- Record default and modified firewall settings.
- Document network configuration.
- Record VM hardware resources.
- Create and verify snapshot.
- Record observation start and end times.
- Preserve scans, logs, and packet captures.
- Document differences from previous Windows runs.

**Expected output:** Comparable measurements across selected Windows versions under equivalent network conditions.

---

## 6. Phase 4 — Mitigation Testing

### Configuration

Apply selected security measures to configurations previously tested.

Potential mitigation measures:

- Network isolation or segmentation.
- More restrictive firewall rules.
- Reduced access to unnecessary services.
- Additional endpoint hardening where supported.

### Measurements

Perform matched before/after comparisons of:

- Externally reachable services.
- Inter-host reachability.
- Allowed and blocked connections.
- Remaining accessible services and identified security exposures.
- Observed suspicious activity, where comparable data exists.

### Documentation

- Record the original configuration.
- Document each mitigation introduced.
- Record modified firewall and network rules.
- Repeat standardized scans.
- Perform controlled reachability tests.
- Preserve before/after evidence.
- Identify remaining observable security exposures.
- Record limitations of the mitigation.

**Expected output:** Evidence of the effectiveness and limitations of selected security controls.

---

## 7. Configuration Change Management

All changes to the experimental environment must be documented.

### Change Log Template

|Date/Time (UTC)|Phase|Endpoint|Configuration Change|Reason|Verification|Evidence|
|---|---|---|---|---|---|---|
|TBD|1|Hardened Linux|Initial configuration|Establish baseline|Pending|TBD|
|TBD|2|Less-secure Linux|Endpoint introduced|Compare exposure|Pending|TBD|
|TBD|3|Legacy Windows|Windows version replaced|Compare OS versions|Pending|TBD|
|TBD|4|Selected endpoint|Mitigation applied|Evaluate protection|Pending|TBD|

### Change Recording Rules

1. Record all intentional configuration changes.
2. Document the configuration before and after each change.
3. Record when changes occur, using UTC.
4. Record which VM or network component was affected.
5. Explain why each change was made.
6. Verify that the intended change took effect.
7. Link supporting evidence where applicable.
8. Record unexpected changes or deviations.
9. Avoid undocumented configuration changes during observation periods.
10. Preserve original experimental configurations using snapshots or other verified recovery methods.

---

## 8. Experimental Consistency

To improve comparability:

- Use standardized measurement procedures.
- Keep observation periods consistent.
- Maintain equivalent exposure conditions where possible.
- Separate controlled test traffic from unsolicited traffic.
- Document configuration differences between operating systems.
- Record monitoring gaps and packet loss.
- Keep raw evidence separate from analysis.
- Restore test environments to known states between relevant experiments.

Changes that cannot be controlled will be documented as potential limitations.

---

## 9. Decisions Still Required

- Confirm exact legacy Windows versions.
- Confirm whether Linux endpoints remain active during Phase 3.
- Determine exact services and exposure settings.
- Confirm network topology and authorization with Frostbyte.
- Determine observation duration for each run.
- Select mitigation measures for Phase 4.
- Define the approved configuration change process.
- Decide where raw logs and captures will be stored.

---

