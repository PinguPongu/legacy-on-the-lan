## Initial State

**Date:** 20 October 2026
**Router:** lab-router
**Platform:** OPNsense 26.1.6_2
**Virtualization:** QEMU

### Network Interfaces

| Interface | Device | IP Address |
|---|---|---|
| LAN | vtnet0 | 192.168.1.1/24 |
| WAN | vtnet1 | Not displayed |

### Configuration Changes

No changes made yet.

### Next Steps

- [ ] Verify interface assignments.
- [ ] Confirm WAN connectivity.
- [ ] Confirm lab network isolation.
- [ ] Access the OPNsense web interface.
- [ ] Back up the initial configuration.
- [ ] Configure logging and monitoring.
- [ ] Review firewall and NAT rules.


--- 


**Router:** lab-router  
**Operating System:** OPNsense 26.1.6_2 (amd64)  
**Virtualization:** QEMU  
**Initial Inspection:** 21 October 2026  
**Configuration Status:** Initial inspection completed; upstream connectivity not verified.

## 1. Initial Network Configuration

### Network Interfaces

|Interface|Device|Address|Status|
|---|---|---|---|
|LAN|`vtnet0`|`192.168.1.1/24`|Previously verified active|
|WAN|`vtnet1`|No IPv4 address observed|No carrier|
|Loopback|`lo0`|`127.0.0.1`|Active|

**Note:** The complete `ifconfig -a` output was not captured. Some interface details remain unverified.

### IPv4 Routing Table

Command:

`netstat -rn -f inet`

|Destination|Gateway|Interface|
|---|---|---|
|`127.0.0.1`|`link#3`|`lo0`|
|`192.168.1.0/24`|`link#1`|`vtnet0`|
|`192.168.1.1`|`link#3`|`lo0`|

**Finding:** No default IPv4 route was present during the inspection.

### DNS Configuration

Command:

`cat /etc/resolv.conf`

Observed configuration:

```
domain internal
nameserver 127.0.0.1
search internal
```

**Finding:** The router is configured to query a local DNS resolver at `127.0.0.1`.

The availability of upstream DNS resolution has not yet been verified.

---

## 2. Firewall Status

Command:

`pfctl -si`

|Property|Observed Value|
|---|---|
|Firewall status|Enabled|
|Uptime|0 days, 16:35:31|
|Debug level|Urgent|
|Current state entries|0|
|State searches|80|
|State inserts|46|
|State removals|46|
|Rule matches|46|

The visible error counters were zero.

**Finding:** The packet filter was enabled and operational at the time of inspection.

No active state entries were present when the command was executed. This does not establish whether firewall rules are correctly configured.

---

## 3. Management Services

### HTTPS

Previously verified:

- Web server: `lighttpd`
    
- Listening port: TCP 443
    
- Listening address: All IPv4 interfaces
    
- Remote accessibility: Not yet verified
    

### SSH

- OpenSSH was initially disabled.
    
- SSH was started temporarily using `service openssh onestart`.
    
- Persistent configuration: Not completed.
    
- SSH key authentication: Not configured or verified.
    
- Remote SSH access: Not verified.
    

---

## 4. Initial Findings

1. The router has a configured LAN network.
    
2. The WAN interface currently reports no carrier.
    
3. No default IPv4 route is present.
    
4. Local DNS resolution is configured through `127.0.0.1`.
    
5. The packet filter is enabled.
    
6. The HTTPS service is listening.
    
7. Management connectivity from the Frostbyte VPN remains unverified.
    

---

## 5. Configuration Change Log

|Date|Action|Result|
|---|---|---|
|20 Oct 2026|Inspected router console and interfaces|Initial LAN and HTTPS settings identified|
|20 Oct 2026|Temporarily started OpenSSH|Service start reported successful; remote access unverified|
|21 Oct 2026|Inspected interfaces, routing, DNS, and firewall|Initial configuration documented|

No permanent network or firewall configuration changes have been confirmed.

---

## 6. Next Steps

- Inspect firewall status.
    
- Inspect IPv4 routing table.
    
- Inspect resolver configuration.
    
- Identify WAN carrier issue.
    
- Confirm virtual WAN adapter connection in the hypervisor.
    
- Verify WAN interface assignment.
    
- Configure approved upstream connectivity.
    
- Verify network isolation.
    
- Establish management access.
    
- Back up the initial router configuration.
    
- Review firewall and NAT rules.
    

## 7. Notes

WAN connectivity must be investigated before enabling Internet-facing experiments.

All changes will be documented and verified before proceeding to the next configuration stage.

---
## WAN Connectivity Investigation — 21 October 2026

**Status:** Investigation in progress  
**Configuration changes:** None

### 1. WAN Interface Inspection

**Command executed:**

`ifconfig vtnet1`

**Observed results:**

|Property|Value|
|---|---|
|Interface|`vtnet1`|
|Assigned role|WAN|
|Interface flags|UP, BROADCAST, RUNNING, SIMPLEX, MULTICAST|
|MTU|1500|
|MAC address|`bc:24:11:79:92:64`|
|IPv4 address|None|
|IPv6 address|`fe80::be24:11ff:fe79:9264%vtnet1`|
|Media|Ethernet autoselect (none)|
|Link status|**No carrier**|

### 2. Network Adapter Identification

**Command executed:**

`sysctl -n dev.vtnet.1.%desc`

**Output:**

```
VirtIO Networking Adapter
```

The WAN interface is recognized as a VirtIO virtual network adapter.

### 3. Findings

- OPNsense recognizes `vtnet1` as its WAN interface.
    
- The interface is administratively enabled (`UP`).
    
- The virtual network adapter is detected by the operating system.
    
- The interface reports `no carrier`, indicating that no active network link is detected.
    
- No IPv4 address is assigned to the WAN interface.
    
- The previous routing-table inspection showed no default IPv4 route.
    

**Preliminary assessment:** The WAN connectivity issue may originate from the virtual network connection or hypervisor configuration. This has not yet been confirmed.

### 4. Next Steps

- Verify WAN interface status.
    
- Identify WAN network adapter type.
    
- Inspect the VM's virtual network adapter configuration.
    
- Confirm the upstream virtual bridge/network.
    
- Verify that the adapter is connected and enabled.
    
- Confirm the intended WAN addressing method.
    
- Recheck carrier status after verifying the connection.
    
- Verify IPv4 addressing and default gateway.
    

### 5. Change Log

|Date|Action|Result|
|---|---|---|
|21 Oct 2026|Executed `ifconfig vtnet1`|WAN reports no carrier and has no IPv4 address|
|21 Oct 2026|Executed `sysctl -n dev.vtnet.1.%desc`|Confirmed VirtIO Networking Adapter|

**No router configuration was changed during these checks.**

---

### Dnsmasq Configuration Inspection — 21 October 2026

**Status:** Investigation ongoing  
**Configuration changes:** None

#### Commands Executed

```
grep -n -A 60 '<dnsmasq>' /conf/config.xml
ps auxww | grep '[d]nsmasq'
```

#### Results

**1. OPNsense XML configuration**

The search for `<dnsmasq>` in `/conf/config.xml` returned no matching output.

This does not necessarily mean Dnsmasq is unconfigured. Its settings may be stored under different configuration elements.

**2. Running Dnsmasq process**

The following process was identified:

```
User: nobody
PID: 89097
Executable: /usr/local/sbin/dnsmasq
PID file: /var/run/dnsmasq.pid
Configuration: /usr/local/etc/dnsmasq.conf
```

#### Findings

- Dnsmasq is running under the `nobody` account.
    
- The process uses `/usr/local/etc/dnsmasq.conf` as its configuration file.
    
- Previous inspection confirmed Dnsmasq listening on UDP port 67.
    
- The configured DHCP address range remains unknown.
    
- Existing DHCP reservations have not yet been identified.
    
- No configuration changes were made.
    

#### Next Steps

- Identify the Dnsmasq process.
    
- Locate its configuration file.
    
- Inspect `/usr/local/etc/dnsmasq.conf`.
    
- Identify the active DHCP range.
    
- Identify configured DHCP reservations.
    
- Confirm DHCP interface bindings.
    
- Finalize IP assignments for experimental VMs.
    
- Verify DHCP operation using a lab endpoint.
    

**Note:** OPNsense may regenerate service configuration files. Any permanent changes should be made through its supported configuration system rather than manually editing generated files.

---

### DHCP Configuration Discovery — 21 October 2026

**Status:** DHCP range identified  
**Configuration changes:** None

**Configuration file:** `/usr/local/etc/dnsmasq.conf`

|Property|Observed configuration|
|---|---|
|DHCP implementation|Dnsmasq|
|LAN interface|`vtnet0`|
|IPv4 address pool|`192.168.1.100–192.168.1.199`|
|Lease duration|86,400 seconds (24 hours)|
|Additional config directory|`/usr/local/etc/dnsmasq.conf.d/`|
|IPv6|DHCPv6/SLAAC configuration present|

#### Findings

- DHCP is configured for the laboratory LAN.
    
- The existing dynamic IPv4 pool does not overlap with the proposed fixed addresses for the experimental VMs.
    
- Further checks are needed to confirm whether any fixed addresses are already assigned.
    
- No configuration changes have been made.
    

#### Next Steps

- Identify DHCP implementation.
    
- Identify DHCP address range.
    
- Check for existing DHCP reservations.
    
- Verify proposed static IP addresses are unused.
    
- Configure fixed addresses or DHCP reservations.
    
- Test DHCP from an experimental VM.

---

### DHCP Reservation Inspection — 21 October 2026

**Status:** Initial directory inspection completed  
**Configuration changes:** None

#### Commands Executed

```
ls -l /usr/local/etc/dnsmasq.conf.d/
grep -r dhcp-host /usr/local/etc/dnsmasq.conf.d/
```

#### Findings

- The Dnsmasq configuration directory contains only a `README` file.
    
- No `dhcp-host` entries were found in the inspected directory.
    
- No existing DHCP reservations were identified through this check.
    
- The presence of reservations in other OPNsense configuration locations has not yet been ruled out.
    
- No configuration changes were made.
    

#### Next Steps

- Inspect additional Dnsmasq configuration directory.
    
- Check for reservations in the main configuration.
    
- Confirm proposed fixed IP addresses are available.
    
- Assign consistent addresses to experimental VMs.
    
- Verify addressing from a lab endpoint.

---

### DHCP Reservation Check — 21 October 2026

**Command:** `grep dhcp-host /usr/local/etc/dnsmasq.conf`

**Result:** No matching entries.

**Findings:**
- No DHCP reservations found in the inspected Dnsmasq files.
- DHCP pool: 192.168.1.100–192.168.1.199.
- Proposed fixed VM addresses remain unassigned.
- No configuration changes made.

---
### Firewall and NAT Inspection — 21 October 2026

**Status:** Initial inspection completed  
**Configuration changes:** None

#### Commands Executed

```
pfctl -sr
pfctl -sn
```

#### Firewall Findings

From the visible firewall rules:

- WAN traffic from certain reserved or special-use address ranges is blocked.
    
- General outbound traffic permission is present.
    
- LAN access to the router's HTTP and HTTPS services is allowed.
    
- A broad IPv4 LAN allow rule is present.
    
- IPv6 LAN traffic is permitted by an observed rule.
    
- Several rules have logging enabled.
    

The complete firewall ruleset has not yet been captured because of console display limitations.

#### NAT Findings

- No WAN port-forwarding rules were visible.
    
- HTTP and HTTPS redirect exclusions exist on LAN.
    
- WAN connectivity remains unavailable.
    

#### Security Considerations

The existing LAN allow rule may permit experimental endpoints to initiate connections beyond the intended lab boundaries once upstream connectivity is established.

Before connecting WAN, the research team must establish appropriate outbound restrictions and confirm isolation from unrelated infrastructure.

#### Next Steps

- Inspect visible firewall rules.
    
- Inspect NAT rules.
    
- Capture the complete firewall ruleset.
    
- Identify existing logging settings.
    
- Define the baseline outbound firewall policy.
    
- Verify management access and recovery procedures.
    
- Review and implement approved restrictions before enabling WAN.
    

**No firewall or NAT settings have been modified.**


---

### Configuration Backup — 21 October 2026

**Status:** Completed

| Backup | Location |
|---|---|
| Active firewall rules | `/root/firewall-rules-before.txt` |
| OPNsense configuration | `/conf/config.xml.pre-lab-setup-20261021` |

- [x] Export firewall rules.
- [x] Back up OPNsense configuration.
- [ ] Test configuration restoration (deferred).

No active firewall settings were modified.

---

### LAN Firewall Rule Inspection — 21 October 2026

**Status:** Initial review completed  
**Configuration changes:** None

#### Command Executed

`pfctl -sr | grep vtnet0`

#### Observed Rules

|Rule category|Observation|
|---|---|
|Source validation|Traffic with unexpected IPv4 source addresses is blocked on LAN|
|DHCP|IPv4 DHCP client/server communication is permitted|
|DHCPv6|Multiple IPv6 DHCP and related communication rules are present|
|HTTP management|TCP port 80 to the router is permitted|
|HTTPS management|TCP port 443 to the router is permitted|
|General IPv4 access|LAN network traffic to any destination is broadly permitted|
|General IPv6 access|IPv6 traffic from LAN is broadly permitted|

#### Security Assessment

The LAN currently contains a general allow rule for outbound IPv4 traffic

---

### Lab Network Topology Verification — 21 October 2026

**Status:** Shared bridge confirmed  
**Configuration changes:** None

#### Verified Configuration

- Router LAN interface: `vtnet0`
    
- Router LAN address: `192.168.1.1/24`
    
- Proxmox bridge: `vmbr9`
    
- Experimental VMs: Connected to `vmbr9`
    
- Windows testing: One Windows version active at a time
    

#### Findings

All experimental VMs share the same Proxmox network bridge.

Traffic between machines on the same subnet can bypass OPNsense, meaning router firewall rules alone may not provide inter-host isolation.

The monitoring VM may require virtual-switch port mirroring or another suitable capture method to observe traffic between experimental endpoints.

#### Planned Actions

- Confirm shared virtual bridge.
    
- Verify isolation from unrelated networks.
    
- Confirm monitoring traffic visibility.
    
- Review outbound firewall restrictions.
    
- Decide how inter-host isolation will be evaluated during Phase 4.
    

No configuration changes were made.


---

### Outbound Access Policy — 21 October 2026

**Status:** Proposed — not implemented

#### Research Requirements
- Ubuntu Desktop should support ordinary web browsing.
- Ubuntu Server should have limited outbound access.
- Legacy Windows should have restricted outbound access.
- Monitoring VM should only access required services.

#### Baseline Containment
- Block unauthorized access to Frostbyte infrastructure.
- Prevent uncontrolled outbound activity from legacy endpoints.
- Maintain required DNS, NTP, and update connectivity.
- Log relevant firewall decisions.
- Review IPv6 access separately.

#### Configuration Changes
None yet.

---

## OPNsense Firewall Configuration — 21 October 2026

### Objective

Configure the lab router to meet Frostbyte's security requirements before connecting the lab to the Internet:

- Block outgoing email traffic on TCP ports 25, 465 and 587.
    
- Deny outbound traffic by default, allowing only necessary services.
    
- Enable logging on blocked traffic.
    

### Environment

|Component|Configuration|
|---|---|
|Router|OPNsense 26.1.6_2|
|LAN interface|`vtnet0` — `192.168.1.1/24`|
|WAN interface|`vtnet1` — disconnected|
|Lab network|Proxmox bridge `vmbr9`|
|Management|`https://192.168.1.1` via Ubuntu Desktop VM|

### Configuration Performed

All firewall changes were made through **OPNsense → Firewall → Rules → LAN**.

Port aliases were created through **Firewall → Aliases**.

|Rule / Alias|Configuration|Purpose|
|---|---|---|
|`BLOCKED_MAIL_PORTS`|TCP 25, 465, 587|Identify prohibited email ports|
|Block outbound SMTP|Block, IPv4+IPv6, logging enabled|Prevent outgoing email|
|Allow DNS|TCP/UDP 53 → `192.168.1.1`|Preserve DNS resolution|
|`LAB_WEB_PORTS`|TCP 80, 443|Identify approved web ports|
|Temporary web access|IPv4 TCP, `LAB_WEB_PORTS`|Permit software downloads and updates|
|Allow management|IPv4 TCP 443 → `192.168.1.1`|Preserve access to OPNsense|
|Default deny outbound|Block all remaining IPv4+IPv6 traffic, logging enabled|Prevent unapproved connections|

The two original rules, **Default allow LAN to any rule** (IPv4 and IPv6), were disabled.

Rules were ordered so that specific blocks and approved exceptions are evaluated before the default-deny rule.

Existing automatically generated DHCP rules were retained.

### Verification

- Confirmed SMTP block rules were active using `pfctl -sr`.
    
- Confirmed the unrestricted IPv4 LAN pass rule was absent from the active PF ruleset.
    
- Verified that the OPNsense web interface remained accessible after the changes.
    
- Attempted to access `http://192.168.1.1:8080` from Ubuntu Desktop.
    
- Confirmed the connection was blocked and recorded in **Firewall → Log Files → Live View**.
    
- Confirmed that OPNsense had automatic DHCPv4 and DHCPv6 rules.
    

### Security Impact

The firewall now applies a **default-deny policy to traffic entering the router from the lab LAN**. Only explicitly permitted services can pass the configured LAN rules.

Blocking SMTP reduces the risk of compromised test machines sending email. Logging provides evidence of denied connection attempts for later analysis.

**Limitations:** Traffic directly between VMs on the same Proxmox bridge may bypass OPNsense. Temporary HTTP/HTTPS access remains enabled for setup. The WAN is still disconnected, so Internet-facing behavior has not yet been fully tested.

### Status

**Configured and locally verified.** Final upstream connectivity tests remain pending and should be coordinated with Frostbyte before WAN activation.