# Legacy on the LAN — Debian Monitoring Station Runbook

**Status:** Proposed procedure; commands not yet executed  
**Target:** Debian 13 monitoring VM (`monitor`), Proxmox/QEMU, 4 vCPU, 8 GB RAM, 80 GB disk  
**Project:** Legacy on the LAN  
**Prepared:** 21 October 2026  
**Operator:** Research team

> **Important:** Execute the stages in order and record the actual outcome of each. Do not run the entire document as one script. Commands assume the existing `researcher` account and access to the Proxmox console. Commands prefixed with `sudo` require sudo privileges; on a root console omit `sudo`. Replace `<MONITOR_IP>`, `<NIC>`, `<ADMIN_IP>`, and `<LAB_VM_IP>` with verified values. Do not type angle brackets literally.

## 0. Design and safety boundaries

We will use **ntopng as the browser dashboard**, **tcpdump/TShark for defensible packet evidence**, and **Nmap for documented service scans**. Add **Suricata** later only after traffic visibility is verified. OPNsense Insight and firewall logs provide complementary gateway observations, not the same thing as packet captures.

The monitoring VM is attached to `vmbr9` on the `192.168.1.0/24` lab LAN. **A VM on the same bridge does not automatically capture other VMs' unicast traffic.** Valid capture options require Frostbyte's approval: virtual-switch traffic mirroring, a dedicated capture interface, or capture on the OPNsense gateway. Until then, ntopng will mainly see traffic transmitted to/from `monitor` plus broadcasts/multicasts.

- Do not enable WAN or public monitoring-dashboard access while setting this up.
- Keep the monitoring machine's management interfaces accessible only within the approved lab / VPN management path.
- Do not capture unrelated networks. Verify permission and evidence-retention arrangements.
- Keep installation/update traffic outside official experimental observation windows.
- Before every major configuration change, record the date, command/configuration, reason, outcome and rollback.
- Do not expose legacy systems solely to test the monitoring software.

## 1. Verify the operating system and network

Run at the VM console:

```bash
cat /etc/os-release
hostnamectl
id researcher
ip -br address
ip route
ip -br link
lsblk
free -h
df -h
```

Confirm the monitoring VM is Debian 13, hostname `monitor`, with the expected resources and a network interface on `vmbr9`. An interface name might be `ens18`; **use the observed name**, not a guessed one.

Test the local router (Internet not required):

```bash
ping -c 3 192.168.1.1
```

After Frostbyte enables **approved outbound-only Internet** for setup:

```bash
ping -c 3 1.1.1.1
getent hosts deb.debian.org
```

If the first external test fails, check gateway/routes; if only DNS fails, troubleshoot resolver configuration. Do not change OPNsense WAN or Proxmox bridges without Frostbyte approval.

**Record:** OS release, VM ID, vCPU/RAM/disk, MAC, IP address, gateway, NIC name and the results of both connectivity tests.

## 2. Update Debian and install the essentials

From the **root** console:

```bash
apt update
apt upgrade
apt install sudo openssh-server ca-certificates curl wget gnupg jq \
  tcpdump tshark nmap python3 python3-venv rsync chrony less vim
```

During Wireshark installation you may be asked whether non-superusers should be able to capture packets. Choose **No initially** (only root capture) to minimize privileges; change this later if needed for a dedicated capture user. Do not run the Wireshark GUI as root.

Check tools:

```bash
command -v sshd
command -v tcpdump
command -v tshark
command -v nmap
python3 --version
```

Record the tool versions for repeatability:

```bash
nmap --version
tcpdump --version
tshark --version
```

## 3. Give the research account administrative access

Run as root:

```bash
usermod -aG sudo researcher
id researcher
```

Log out and back in as `researcher`, then check:

```bash
sudo -v
sudo whoami
```

Expected output of `sudo whoami`: `root`. Do not alter `root` or delete the Proxmox console recovery method.

## 4. SSH server — install, enable, verify

```bash
sudo systemctl enable --now ssh
systemctl status ssh --no-pager
sudo sshd -t
sudo ss -lntp | grep ':22'
```

If the SSH service is not running, check:

```bash
sudo journalctl -u ssh --no-pager -n 60
```

### 4.1 Public-key login

On **your own computer** (not the monitor VM), use an existing key or generate one:

```bash
ssh-keygen -t ed25519 -C 'legacy-on-the-lan-monitor'
cat ~/.ssh/id_ed25519.pub
```

Copy **only the public key**. In the Proxmox console logged in as `researcher`:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste the full `ssh-ed25519 ...` public-key line into that file, then:

```bash
chmod 600 ~/.ssh/authorized_keys
```

From an **authorized machine with a route to the lab**:

```bash
ssh -i ~/.ssh/id_ed25519 researcher@<MONITOR_IP>
```

A VPN connection does **not** guarantee a route to `192.168.1.0/24`. If that route is unavailable, SSH from another approved VM on the lab LAN or ask Frostbyte about a management/jump path. **Do not publish SSH to the Internet.**

### 4.2 Harden SSH after testing key login

Keep the Proxmox console open. Only **after key-based login is verified**, make a backup and add a drop-in:

```bash
sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.before-lab
sudo nano /etc/ssh/sshd_config.d/90-legacy-lan.conf
```

Enter:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
```

Validate, then apply:

```bash
sudo sshd -t
sudo systemctl reload ssh
```

**Open a second SSH connection** to verify it works before closing the first. If not, restore the configuration from the Proxmox console. Note that effective settings may be affected by earlier `sshd_config.d` files: inspect using `sudo sshd -T` and adjust accordingly.

## 5. Time synchronization

Accurate timestamps matter for comparing packet captures, authentication events and firewall logs.

```bash
timedatectl
systemctl status chrony --no-pager
chronyc tracking
chronyc sources -v
```

Keep the OS clock synchronized to a reachable **approved** time source. Configure Debian, OPNsense and the other VMs consistently. If no approved NTP endpoint is available while offline, record clock offsets; do not claim timestamps are synchronized. For event records, use UTC:

```bash
timedatectl set-timezone UTC
date -u
```

## 6. Create evidence storage with safe ownership

```bash
sudo mkdir -p /opt/legacy-lan/{captures,scans,logs,analysis,scripts,manifests}
sudo chown -R researcher:researcher /opt/legacy-lan
sudo chmod 750 /opt/legacy-lan
ls -ld /opt/legacy-lan/*
df -h /opt/legacy-lan
```

**Protect captures:** they can contain credentials, IP addresses or content. Keep data off the disposable VM in approved storage; do not put raw sensitive PCAPs in a synced Obsidian vault. Use hashes and an evidence manifest:

```bash
sha256sum /opt/legacy-lan/captures/<CAPTURE_FILENAME>
```

### 6.1 Recording template

For each observation, record:

| Field | Value |
|---|---|
| Experiment ID / phase | TBD |
| Endpoint / IP | TBD |
| Collection point and NIC | TBD |
| Start/end (UTC) | TBD |
| Actual valid capture duration | TBD |
| Commands/tool versions | TBD |
| Controlled test traffic excluded? | TBD |
| Dropped packets / capture gaps | TBD |
| Filename and SHA256 | TBD |
| Operator and deviations | TBD |

## 7. Packet capture — test before trusting the data

Identify the NIC:

```bash
ip -br link
sudo tcpdump -D
```

Test only traffic visible on the monitor's own interface, substituting its real name:

```bash
sudo tcpdump -i <NIC> -nn -c 25
```

Save a **short** pilot capture:

```bash
sudo tcpdump -i <NIC> -nn -s 0 -G 60 -W 1 \
  -w /opt/legacy-lan/captures/pilot-%Y%m%d-%H%M%S.pcap
```

If file ownership is root, transfer it to the approved evidence account after capture:

```bash
sudo chown researcher:researcher /opt/legacy-lan/captures/pilot-*.pcap
```

Read it:

```bash
tshark -r /opt/legacy-lan/captures/<PILOT_FILENAME> -q -z io,stat,10
```

For **repeated bounded** captures, use time/size rotation and monitor disk space. Example: 10 files of up to 100 MB each (captures stop after the ring fills):

```bash
sudo tcpdump -i <NIC> -nn -s 0 -C 100 -W 10 \
  -w /opt/legacy-lan/captures/phase-pilot.pcap
```

Run only during an authorized test period. Stop with `Ctrl+C`; record the packet-drop summary. This is **not** automatic permanent surveillance.

### 7.1 Prove visibility

Perform a known, approved connection between two other lab VMs and see whether it appears in the monitor capture. If not, that is expected on an unmirrored bridge. **Do not interpret an empty capture as no network activity.** Ask Frostbyte to provide either:

1. A mirrored/SPAN capture feed into a dedicated monitor NIC (preferred for seeing same-subnet traffic), or
2. Gateway packet captures/flow export from OPNsense, noting that same-subnet traffic bypasses OPNsense, or
3. Endpoint-level capture agents/captures for specific tests.

Record exactly which interfaces, directions and dropped/filtered packets each sensor can see.

## 8. Nmap — controlled, tagged service measurements

Only scan the approved lab addresses. Run from `monitor` and save results with the experiment ID:

```bash
nmap -sT -sV -oA /opt/legacy-lan/scans/pilot-ubuntu <LAB_VM_IP>
```

Note the scanner IP, version, protocol/port scope and UTC start/end. Tag scan intervals so these connection attempts can be excluded from unsolicited/hostile activity totals. An accessible port is evidence of reachability, **not** proof of vulnerability.

## 9. Install ntopng dashboard (after basic capture works)

ntopng has an official Debian 13 repository. The published Debian 13 instructions require Debian `contrib` to be enabled in the relevant APT sources. Check which source-file format this install uses:

```bash
cat /etc/os-release
ls /etc/apt/sources.list.d/
cat /etc/apt/sources.list
```

If your Debian 13 sources are in `/etc/apt/sources.list.d/debian.sources`, inspect that file and add `contrib` to the appropriate `Components:` lines **only if absent**. Make a backup before editing. Do not blindly overwrite repository files.

```bash
sudo apt update
sudo apt install lsb-release wget gnupg apt-transport-https ca-certificates
cd /tmp
wget https://packages.ntop.org/apt/trixie/all/apt-ntop.deb
sudo apt install ./apt-ntop.deb
sudo apt update
sudo apt install ntopng
```

**Warning:** The ntop page publishes Debian 13 packages under its current `/apt/trixie/` repository. Confirm package origin and dependencies before accepting the APT changes:

```bash
apt-cache policy ntopng
```

### 9.1 Configure capture source

Find the installed config file and check current service state:

```bash
systemctl status ntopng --no-pager
sudo find /etc -maxdepth 3 -name ntopng.conf
sudo ss -lntp | grep -E ':3000|:3001'
```

Typical configuration file: `/etc/ntopng/ntopng.conf`. Back it up before editing:

```bash
sudo cp -a /etc/ntopng/ntopng.conf /etc/ntopng/ntopng.conf.before-lab
sudo nano /etc/ntopng/ntopng.conf
```

**Example only** — select the *verified* actual capture NIC (which may be a second mirrored NIC, not the management NIC):

```text
--interface=<NIC>
--local-networks=192.168.1.0/24
```

Do not replace existing unrelated package options without reading them. Each option is one line, formatted `--option=value`. The default monitor interface will not see other VMs' normal unicast traffic unless mirroring or export is configured.

```bash
sudo systemctl enable --now ntopng
sudo systemctl restart ntopng
sudo systemctl status ntopng --no-pager
sudo journalctl -u ntopng --no-pager -n 50
sudo ss -lntp | grep -E ':3000|:3001'
```

### 9.2 Open the dashboard securely

The current ntop documentation says some installed packages default to **HTTPS port 3001**, whereas older releases/documentation describe HTTP port 3000. Follow the actual `ss` result. On Ubuntu Desktop **inside the approved lab**, visit either:

- `https://<MONITOR_IP>:3001` (preferred when enabled), or
- `http://<MONITOR_IP>:3000` (only if that is what the installation actually serves).

If prompted to change a default login password, do it immediately and use a strong unique password. Do not disable authentication or publish the dashboard on WAN.

You can use the Ubuntu Desktop browser in **full-screen mode (F11)** as a kiosk display. No graphical desktop is needed on the Debian monitor. Avoid an unattended, automatically logged-in dashboard with administrator privileges.

Optional: when SSH management is working, use a tunnel from an approved management laptop rather than opening the dashboard widely:

```bash
ssh -L 3001:127.0.0.1:3001 researcher@<MONITOR_IP>
```

Then open `https://127.0.0.1:3001` locally (certificate warnings may appear for a self-signed certificate). This works only if the server listens on localhost and the VPN/management route permits SSH.

### 9.3 Verify dashboard validity

- [ ] Dashboard authenticates and loads.
- [ ] Correct observed capture interface is selected.
- [ ] A known test connection appears in the expected view.
- [ ] Source/destination and time align with raw PCAP evidence.
- [ ] Capture gaps and sensor blind spots are documented.
- [ ] No unsupported claim that dashboard equals a full intrusion detector.

## 10. Router-side observations: OPNsense Insight and firewall logs

Once the router web interface is accessible from an approved LAN endpoint:

1. Back up the router configuration (an initial `/conf/config.xml` backup was previously created).
2. Inspect **Reporting → NetFlow** exporter options. Enable *local capture* only for the approved interfaces/directions necessary to support **Reporting → Insight**.
3. In **Reporting → Insight**, inspect flow totals and CSV export.
4. In firewall live log, verify which blocked/allowed rules actually log events.
5. Record whether data is ingress, egress, or both. **Ingress + egress can double-count traffic due to NAT**, so don't blindly sum both.

OPNsense gateway capture covers **traffic crossing the router**, not direct same-subnet conversations. The firewall log's number of records is not necessarily a number of unique connection attempts.

## 11. Optional: Suricata IDS (only after pilot visibility is confirmed)

If time/resources permit:

```bash
sudo apt update
sudo apt install suricata suricata-update
suricata --build-info
sudo suricata-update
```

Inspect and back up `/etc/suricata/suricata.yaml`, then select the *verified capture interface* in its capture configuration and define `HOME_NET` as the actual lab subnet (`192.168.1.0/24`). **Do not guess a capture source** or enable IPS/inline blocking; initially operate in passive detection mode only.

Validate configuration before starting:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
sudo systemctl enable --now suricata
sudo systemctl status suricata --no-pager
sudo tail -n 30 /var/log/suricata/suricata.log
```

Depending on the Debian package/service configuration, further interface settings may be needed in `/etc/default/suricata` or the service definition. Check `systemctl cat suricata` instead of assuming that editing YAML alone selected the correct live interface.

Alerts/events, if enabled, are generally recorded in `/var/log/suricata/eve.json`:

```bash
sudo test -f /var/log/suricata/eve.json && \
  sudo tail -n 3 /var/log/suricata/eve.json
```

Suricata alerts identify signatures/heuristics, **not confirmed compromises**. Keep this optional until you have validated the underlying capture feed, storage and performance. Test with benign known lab traffic.

## 12. Data quality, disk and health checks

```bash
df -h /opt/legacy-lan
free -h
uptime
ip -s link
sudo journalctl -p warning -b --no-pager -n 50
```

Check for capture packet loss, unexpected CPU usage, dropped link packets, disk pressure and clock drift. PCAPs may grow rapidly; don't run an unbounded capture indefinitely. Create a per-phase run manifest and preserve original evidence after acquisition. Consider compression/archiving only after hashing the original files.

## 13. Clean snapshot and acceptance test

After configuration, ask Frostbyte/Proxmox to capture a snapshot named `monitor-clean-configured` (or create it yourselves if permitted). Snapshots aren't substitutes for evidence backups.

**Acceptance checks before Phase 1:**

- [ ] Debian version and VM resources verified.
- [ ] Local LAN networking works.
- [ ] Temporary installation Internet access has been reviewed/removed as needed.
- [ ] SSH key login verified via approved route.
- [ ] SSH root/password restrictions reviewed and management access retained.
- [ ] Clock synchronized or offsets documented.
- [ ] ntopng running and accessible from approved browser.
- [ ] Capture source validated with known test traffic.
- [ ] Packet data written, read back, hashed and stored safely.
- [ ] Nmap produces saved outputs using documented parameters.
- [ ] OPNsense monitoring/export role and visibility recorded.
- [ ] Snapshot and restoration plan confirmed.

## 14. Change log template

| Date/time (UTC) | Host | Command/change | Reason | Observed result | Rollback / evidence |
|---|---|---|---|---|---|
| TBD | monitor | OS installed | Monitoring station | To verify | VM console / inventory |
| TBD | monitor | Install essential packages | Prepare collection tools | Pending | apt log / versions |
| TBD | monitor | Configure SSH keys | Secure administration | Pending | SSH connection test |
| TBD | monitor | Install/configure ntopng | Web dashboard | Pending | Service status / screenshot |
| TBD | monitor | Verify capture visibility | Valid research measurements | Pending | PCAP + controlled test |

## 15. Primary technical references (checked 21 October 2026)

- Debian SSH: https://wiki.debian.org/SSH
- Wireshark Debian capture permissions: https://github.com/wireshark/wireshark/blob/master/packaging/debian/README.Debian
- ntop repository instructions for Debian 13: https://packages.ntop.org/apt/HEADER.html
- ntopng installation: https://www.ntop.org/guides/ntopng/installation.html
- ntopng configuration file: https://www.ntop.org/guides/ntopng/how_to_start/configuration_file.html
- ntopng interface selection: https://www.ntop.org/guides/ntopng/cli_options/cli_options.html
- OPNsense NetFlow and Insight: https://docs.opnsense.org/manual/netflow.html and https://docs.opnsense.org/manual/how-tos/insight.html
- Suricata documentation: https://docs.suricata.io/

**Research note:** Commands and example settings above are a planned procedure based on the expected Debian 13 lab. They must be verified against actual package versions, interface names, network policies and test results. Nothing in this document establishes that any installation, capture or detection has already succeeded.
