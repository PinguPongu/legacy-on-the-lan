
Status: Proposed; infrastructure details require confirmation.

## Topology to document

Draw Internet → Frostbyte boundary firewall/router → isolated experimental network → experimental VMs. Add monitoring location, management access, public/private addresses, NAT mappings, and any segment boundaries once confirmed.

## VM inventory to complete

| Role                    | OS/version         | Resources                    | Services and controls                                  | Snapshot |
| ----------------------- | ------------------ | ---------------------------- | ------------------------------------------------------ | -------- |
| Hardened Linux          | Ubuntu 26.04.1 LTS | 2 vCPU, 4 GB RAM, 40 GB disk | Updates, firewall, Fail2ban, minimal services, logging | TBD      |
| Less-secure Linux       | Linux Mint 22.3    | 2 vCPU, 4 GB RAM, 40 GB disk | Precisely document differences from baseline           | TBD      |
| Legacy Windows variants | 7 / 8.1 / 10       | TBD                          | Record each version and configuration                  | TBD      |
| Monitoring/test source  | Ubuntu 26.04.1 LTS | 4 vCPU, 8 GB RAM, 70 GB disk | Confirm visibility and approved scope                  | TBD      |

## Questions for Frostbyte

- [ ] What systems, addresses, activities, and time periods are authorized?
- [ ] Which inbound services can be exposed, and how do routing/NAT and upstream filtering work?
- [ ] Can the monitor observe all relevant traffic, including inter-host traffic?
- [ ] How are the lab, management network, personal systems, and production systems isolated?
- [ ] What outbound restrictions prevent unintended traffic leaving vulnerable hosts?
- [ ] Who can stop exposure immediately, and what are the incident notification arrangements?
- [ ] Are snapshots, restoration, storage, and public IP continuity available?

## Configuration and recovery tasks

- [ ] Record addresses, routes, NAT, firewall rules, services, accounts, and tool versions.
- [ ] Verify isolation and management access before exposure.
- [ ] Create known starting snapshots and document restoration.
- [ ] Define stop conditions such as unexpected outbound activity or containment failure.
- [ ] Verify how to disconnect the lab and preserve evidence before restoration.
