**Project:** Legacy on the LAN  
**Status:** Draft — Pending Frostbyte Approval  
**Location:** 04 - Lab Environment  
**Last reviewed:** TBD

## 1. Purpose

This document defines the safety procedures and operational restrictions for the Legacy on the LAN experimental environment.

The research involves testing modern, less-hardened, and unsupported operating systems in a controlled virtual network.

The objective is to ensure that experiments can be performed without affecting unrelated systems, networks, or services.

All security and network configurations must be verified before official testing begins.

---

## 2. Authorization and Scope

The experimental environment is expected to be provided by Frostbyte.

Before testing, the research team must confirm written authorization covering the experimental activities.

### Authorization Checklist

- Obtain confirmation of permission to use the laboratory environment.
- Identify the responsible Frostbyte contact.
- Confirm the approved experimental network or VLAN.
- Document approved IP address ranges.
- Confirm whether public Internet exposure is permitted.
- Confirm which protocols and ports may be exposed.
- Confirm authorization for internal and external network scanning.
- Confirm permission to capture and analyze network traffic.
- Confirm permission to deploy unsupported operating systems.    
- Agree on escalation and emergency shutdown procedures.


### Authorization Details

|Item|Information|
|---|---|
|Infrastructure provider|Frostbyte|
|Authorized contact|TBD|
|Approval reference/date|TBD|
|Approved network/VLAN|TBD|
|Approved IP ranges|TBD|
|Permitted Internet exposure|TBD|
|Permitted testing activities|TBD|
|Approved observation period|TBD|
|Emergency contact|TBD|

**Rule:** No Internet-exposed experiments will begin until the relevant permissions and boundaries are confirmed.

---

## 3. Network Isolation

### Objective

Prevent experimental endpoints from accessing or affecting unrelated infrastructure.

All virtual machines will operate inside a dedicated experimental network.

### Planned Isolation Measures

- Use a dedicated VLAN or isolated virtual network.
- Prevent unauthorized communication with Frostbyte production infrastructure.
- Prevent access to personal devices and unrelated networks.
- Restrict access to hypervisor management interfaces.
- Allow only explicitly approved inbound traffic.
- Control communication between experimental endpoints.
- Maintain an emergency method for disconnecting the entire lab.

### Isolation Verification Checklist

- Verify the assigned experimental network.
- Confirm that no unintended routes to unrelated networks exist.
- Verify separation from hypervisor management traffic.
- Test expected allowed connections.
- Test that prohibited connections are blocked.
- Confirm that the restrictions remain effective after VM restarts.
- Document the results of isolation verification.
- Confirm the emergency network-disconnection procedure.

### Isolation Configuration

| Component                     | Planned Configuration |
| ----------------------------- | --------------------- |
| Experimental network          | TBD                   |
| VLAN / Virtual switch         | TBD                   |
| Gateway                       | TBD                   |
| Allowed inbound traffic       | TBD                   |
| Allowed outbound traffic      | TBD                   |
| Inter-host communication      | TBD                   |
| Management network separation | TBD                   |
| Emergency isolation mechanism | TBD                   |

**Important:** An isolated VLAN does not necessarily provide complete isolation if routing or firewall rules still permit communication with other networks.

---

## 4. Outbound Network Restrictions

### Objective

Reduce the risk that a compromised experimental machine could communicate with or attack systems outside the authorized laboratory.

### Proposed Policy

Use a default-deny outbound policy for experimental endpoints.

Allow exceptions only when necessary for an approved research purpose.

Potential exceptions include:

- Approved DNS resolution.
- Required system updates during preparation.
- Time synchronization with an approved source.
- Communication with authorized monitoring infrastructure.

Unsupported Windows endpoints should not have unrestricted outbound Internet access.

Where possible, required updates and installation packages should be obtained before testing, and unnecessary outbound connectivity should remain disabled.

### Outbound Restriction Checklist

- Define permitted outbound destinations.
- Define permitted outbound ports and protocols.
- Configure outbound firewall restrictions.
- Verify that prohibited outbound traffic is blocked.
- Confirm necessary monitoring traffic is permitted.
- Log denied outbound connection attempts where feasible.
- Document all outbound exceptions.
- Remove temporary preparation exceptions before experiments.

### Outbound Rules

|Source|Destination|Protocol/Port|Purpose|Status|
|---|---|---|---|---|
|Experimental VMs|Unapproved Internet destinations|All|Prevent unauthorized outbound activity|Proposed deny|
|Experimental VMs|Production/management networks|All|Prevent access to unrelated infrastructure|Proposed deny|
|Approved Linux VMs|Approved update source|TBD|Preparation and updates|TBD|
|Experimental VMs|Approved time source|TBD|Time synchronization|TBD|
|Experimental VMs|Monitoring infrastructure|TBD|Logging or telemetry|TBD|

**Note:** Outbound restrictions may influence some network activity. Their configuration must therefore remain consistent across comparable experimental runs unless outbound controls are themselves being evaluated.

---

## 5. Stop Conditions

### Objective

Define situations in which an experiment must be paused or terminated to protect the environment and preserve evidence.

### Immediate Stop Conditions

Testing must stop if any of the following occur:

- A test VM attempts or establishes unauthorized communication with unrelated infrastructure.
- Evidence indicates that an endpoint may have been compromised.
- Unexpected outbound scanning, exploitation, or suspicious automated traffic is detected.
- Network isolation or containment controls fail.
- An experimental system causes unexpected disruption to shared infrastructure.
- Monitoring fails in a way that prevents safe observation.
- Frostbyte requests that testing stop.
- The experiment operates outside its authorized scope.

### Emergency Response Procedure

1. Stop the active experiment.
    
2. Isolate the affected VM or experimental network using the approved isolation mechanism.
    
3. Notify the responsible Frostbyte contact if required.
    
4. Record the time and reason for stopping.
    
5. Preserve relevant logs and available evidence without unnecessarily interacting with a potentially compromised machine.
    
6. Document the incident and actions taken.
    
7. Obtain approval before resuming the experiment.
    

### Incident Record

|Field|Information|
|---|---|
|Date and time (UTC)||
|Experiment / Phase||
|Affected VM||
|Triggering event||
|Evidence reference||
|Immediate action taken||
|Person notified||
|Recovery action||
|Authorization to resume||

---

## 6. VM Recovery and Restoration

### Objective

Ensure experimental systems can be returned to known configurations between tests or after unexpected events.

### Snapshot Strategy

Create baseline snapshots after installation and approved configuration, but before official exposure.

Each VM should have a clearly named recovery point.

### Proposed Snapshot Names

|VM Role|Snapshot Name|
|---|---|
|Hardened Linux|`Clean-Hardened-Linux`|
|Less-secure Linux|`Clean-LessSecure-Linux`|
|Legacy Windows version 1|`Clean-Windows-01`|
|Legacy Windows version 2|`Clean-Windows-02`|
|Monitoring VM|`Clean-Monitoring`|

Actual snapshot names and identifiers will be recorded in [[VM Inventory]].

### Recovery Procedure

1. Stop the experiment and disconnect the affected VM from the experimental network.
2. Preserve required evidence in approved storage.
3. Confirm whether an incident review is required before restoration.
4. Restore the approved baseline snapshot or rebuild the VM from trusted installation media.
5. Verify the restored OS and security configuration.
6. Verify network isolation and firewall rules.
7. Confirm monitoring functionality.
8. Record the restoration and verification results.
9. Reconnect the VM only after authorization and verification.

### Recovery Checklist

- Baseline snapshots created.
- Snapshot restoration tested.
- Recovery procedures documented.
- Evidence storage separated from disposable VMs.
- Emergency network isolation tested.
- Configuration verified following restoration.
- Recovery events recorded.

**Important:** Snapshots provide restoration points but are not a substitute for backups or evidence preservation. For suspected compromise, restoring a snapshot does not by itself verify that the broader environment is safe.

---

## 7. Evidence Handling

Experimental data may contain IP addresses, system logs, traffic metadata, and other potentially sensitive information.

### Evidence Rules

- Store evidence only in approved locations.
- Restrict access to authorized research participants.
- Avoid collecting unnecessary personal or unrelated traffic.
- Do not publish unredacted sensitive network information.
- Preserve original evidence files when possible.
- Record acquisition timestamps and experiment identifiers.
- Use file hashes where evidence integrity needs to be demonstrated.
- Follow Frostbyte's requirements for retention and deletion.

### Evidence Storage

**Approved storage location:** TBD  
**Authorized access:** TBD  
**Retention period:** TBD  
**Deletion procedure:** TBD

---

## 8. Pre-Experiment Safety Checklist

All mandatory conditions must be satisfied before official data collection.

- Written authorization confirmed.
- Approved scope documented.
- Experimental network isolated.
- Internet exposure rules reviewed.
- Outbound restrictions configured and verified.
- Access to unrelated networks blocked.
- Emergency isolation procedure tested.
- Snapshots created and restoration tested.
- Monitoring and logging operational.
- Stop conditions understood by the research team.
- Evidence storage approved.
- Frostbyte contact information recorded.

**Experiment readiness:** NOT YET VERIFIED

---

## 9. Open Questions for Frostbyte

- Will we receive a dedicated VLAN or isolated virtual switch?
- How will inbound Internet traffic reach the test VMs?
- Which inbound ports and protocols are permitted?
- Can outbound traffic be blocked by default?
- Can we monitor traffic at the gateway or virtual switch?
- Who can immediately disconnect the experimental network?
- Who is responsible for investigating unexpected activity?
- Are there infrastructure-wide security controls that may influence our measurements?
- Where should potentially sensitive packet captures and logs be stored?
- Who must authorize restarting experiments after a safety incident?

---

## 10. Approval and Review

|Role|Name|Date|Approval Status|
|---|---|---|---|
|Researcher|TBD|TBD|Pending|
|Research supervisor|TBD|TBD|Pending|
|Frostbyte representative|TBD|TBD|Pending|

This document must be reviewed before official experimental activities begin.

