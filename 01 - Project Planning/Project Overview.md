# Project Overview — Legacy on the LAN

## Primary Reference

**Research proposal:** _Evaluating the Security Impact of Outdated and Unhardened Endpoints_

**File:** `/home/gudni/Downloads/pprosal.pdf`

The research proposal defines the project's scope, objectives, and overall experimental approach. Supporting documentation provides additional planning context, but methodological decisions remain subject to review and refinement.

## Research Question

How does introducing endpoints with different security configurations and operating-system ages affect the attack surface and observed hostile activity of a small Internet-exposed network?

## Research Aim

To investigate how endpoint security configurations and operating-system age influence network exposure and observed hostile activity, and to evaluate whether basic defensive controls can reduce the associated security risks.

The research will use a controlled virtualized environment to compare exposed services, network activity, and the effects of selected security controls.

## Experimental Design

The research consists of four experimental phases.

### Phase 1 — Hardened Linux Baseline

Establish a baseline using a modern, security-hardened Linux endpoint.

The system will be configured with:

- Current security updates.
    
- A restrictive firewall configuration.
    
- Fail2ban.
    
- Minimal exposed services.
    
- Network and system logging.
    

This phase establishes reference measurements for subsequent comparisons.

### Phase 2 — Unhardened Linux Endpoint

Introduce a less-secure Linux endpoint into the experimental network.

Repeat the baseline measurements to evaluate changes in exposed services, network activity, and overall attack surface.

### Phase 3 — Legacy Windows Comparison

Evaluate multiple unsupported Windows versions individually under equivalent network conditions.

**Each Windows system will be tested separately, using the same isolated network environment rather than running the legacy Windows systems simultaneously.**

The testing procedure will aim to maintain:

- The same isolated network segment.
    
- Equivalent Internet exposure and firewall rules.
    
- Consistent network addressing or endpoint placement.
    
- Identical monitoring and data-collection methods.
    
- Comparable observation periods.
    
- Consistent scanning and evaluation procedures.
    

Only one legacy Windows test machine will be active at a time.

The operating-system version will be the primary variable of interest. Any differences in default services, installed updates, supported protocols, or system configuration will be documented.

This approach is intended to improve comparability between Windows versions while limiting the influence of changes to the surrounding network environment.

### Phase 4 — Mitigation Evaluation

Implement selected defensive controls and repeat relevant measurements under comparable conditions.

Potential controls include:

- Network segmentation or client isolation.
    
- Firewall restrictions.
    
- Reduced service exposure.
    
- Other basic endpoint or network hardening measures.
    

The objective is to assess whether these controls measurably reduce unnecessary network exposure or permitted communication.

Where possible, mitigation results will be compared directly against the corresponding unmitigated configuration.

## Measurement Strategy

The study will distinguish between three categories of observations:

**1. Attack Surface**

- Reachable ports.
    
- Exposed network services.
    
- Service and protocol versions.
    
- Potentially vulnerable configurations.
    

**2. Observed Network Activity**

- Incoming connection attempts.
    
- Unique external source addresses.
    
- Failed authentication attempts.
    
- Suspicious or potentially hostile network activity.
    

**3. Defensive Effectiveness**

- Blocked and permitted connections.
    
- Changes in externally reachable services.
    
- Host-to-host reachability.
    
- Differences between mitigated and unmitigated configurations.
    

Measurements will be recorded consistently across experimental phases wherever technically feasible.

## Experimental Environment

Frostbyte is expected to provide an isolated, disposable virtual-machine environment.

Before experiments begin, the following must be confirmed:

- Authorization and permitted testing scope.
    
- Internet exposure and routing configuration.
    
- Network isolation from unrelated systems.
    
- Monitoring and logging capabilities.
    
- VM restoration and containment procedures.
    

The use of unsupported or intentionally less-secure operating systems will be restricted to the authorized experimental environment.

No official experiments have yet been verified as completed.

## Methodological Considerations and Limitations

- **Temporal variability:** Internet activity varies over time, potentially affecting comparisons between observation periods.
    
- **Endpoint count:** Adding endpoints in earlier phases changes the number of systems as well as their security configurations.
    
- **Windows comparability:** Legacy Windows systems will be tested individually under equivalent network conditions to reduce environmental differences.
    
- **Operating-system differences:** Windows versions may differ in installed services, protocol support, patch availability, and default configurations. These differences will be documented rather than attributed solely to OS age.
    
- **Exposure versus exploitation:** Exposed services and suspected vulnerabilities do not necessarily demonstrate successful exploitation.
    
- **Observed activity:** Suspicious connections do not automatically establish malicious intent or compromise.
    
- **Mitigation scope:** Reduced network reachability does not necessarily eliminate underlying operating-system vulnerabilities.
    
- **Experimental uncertainty:** Successful compromise is not required or guaranteed.
    

## Research Status

**Current stage:** Preparation and experimental design.

**Next priorities:**

1. Finalize hypotheses, variables, and measurements.
    
2. Define the standardized testing procedure for legacy Windows systems.
    
3. Confirm the experimental environment with Frostbyte.
    
4. Establish monitoring, evidence collection, and documentation procedures.
    
5. Conduct a pilot test before official data collection.
    

## Related Documentation

- [[Dark Arts/Legacy on the Lan/01 - Project Planning/Research Design]]
    
- [[Preparation Checklist]]
    
- [[Research Methodology]]
    
- [[Network Topology]]
    
- [[VM Inventory]]
    
- [[Phase 3 - Legacy Windows]]