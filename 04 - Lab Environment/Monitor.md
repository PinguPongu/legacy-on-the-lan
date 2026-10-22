# Monitoring VM Configuration

**Project:** Legacy on the LAN  
**Status:** Initial setup in progress

## System Information

|Property|Value|
|---|---|
|Hostname|monitor|
|Operating System|Debian (exact version to verify)|
|CPU|TBD|
|RAM|TBD|
|Storage|Approximately 80 GB (verify)|
|Hypervisor|Proxmox/QEMU|
|Network|vmbr9|
|IP Address|TBD|
|Gateway|192.168.1.1|
|SSH|Not installed|

## Planned Tools

- OpenSSH Server
    
- tcpdump
    
- TShark
    
- Nmap
    
- Python 3
    
- Relevant log analysis utilities
    

## Evidence Directory

`/opt/legacy-lan/`

Subdirectories:

- captures/
    
- scans/
    
- logs/
    
- scripts/
    
- analysis/
    

## Setup Checklist

- Verify operating system.
    
- Verify allocated resources.
    
- Verify IP address and network connectivity.
    
- Create evidence directories.
    
- Verify directory permissions.
    
- Check available monitoring tools.
    
- Install missing tools when Internet becomes available.
    
- Configure SSH.
    
- Confirm packet capture permissions.
    
- Verify traffic visibility.
    
- Configure time synchronization.
    
- Create clean VM snapshot.
    
- Test evidence collection.
    

## Configuration Changes

|Date|Change|Result|
|---|---|---|
|21 October 2026|Initial Debian installation|Installed; exact release to verify|

## Notes

The monitoring machine will be used to collect network traffic, perform authorized service discovery, and support analysis across experimental phases.

No official experimental data collection has started.