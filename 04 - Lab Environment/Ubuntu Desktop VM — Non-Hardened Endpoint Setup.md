
**Date:** 22 October 2026  
**Project:** Legacy on the LAN  
**Role:** Standard, intentionally less-secure workstation  
**Status:** Installation configured; completion and network connectivity not yet verified

## 1. Purpose

This VM represents an everyday Ubuntu Desktop workstation with intentionally weak account security.

It will be compared against the hardened Ubuntu Server to investigate differences in network exposure, available services, and security-related activity.

## 2. Installation Configuration

The Ubuntu Desktop installer was used with the following settings:

|Setting|Configuration|
|---|---|
|Operating system|Ubuntu Desktop (exact version not yet verified)|
|Installation type|Normal installation|
|Download updates during installation|Enabled|
|Third-party software|Disabled|
|Full name|María|
|Hostname|`myPC`|
|Username|`maya`|
|Password|Intentionally weak (`password123`)|
|Login method|Automatic login|
|Active Directory|Disabled|

Normal installation was chosen to provide a typical desktop environment with a web browser, office applications, utilities, and media software.

The update option was selected, although successful downloading of updates has not been verified because the lab's WAN connection was previously disconnected.

## 3. Deliberately Weak Configuration

The following choices were made intentionally:

- A predictable local account password was selected.
    
- Automatic login was enabled.
    
- No additional system-hardening measures were configured during installation.
    
- A normal desktop software installation was selected instead of a minimal installation.
    

These settings represent weak local account security but **do not automatically imply that the machine exposes vulnerable network services**. Actual network exposure must be measured during the experiments.

The VM is intended to remain within the controlled Frostbyte lab environment.

## 4. Network Environment

The lab uses:

- **Virtualization platform:** Proxmox
    
- **Virtual network:** `vmbr9`
    
- **Lab subnet:** `192.168.1.0/24`
    
- **OPNsense gateway:** `192.168.1.1`
    

The VM's assigned IP address, gateway, and DNS configuration have not yet been verified.

## 5. Next Steps

- Complete installation and reboot.
    
- Verify the installed Ubuntu version.
    
- Confirm the VM's network interface and IP address.
    
- Verify connectivity to OPNsense.
    
- Record installed packages and listening services.
    
- Capture an initial Nmap scan as baseline evidence.
    
- Take a Proxmox snapshot before experiments.
    
- Avoid connecting real personal accounts or storing real personal data.
    

## 6. Research Notes

The non-hardened Ubuntu Desktop will serve as a comparison endpoint, rather than as a deliberately compromised machine.

The effect of weak credentials, automatic login, desktop applications, and absent additional hardening will be discussed separately from measured network exposure.

**Important:** The hardened Ubuntu Server and Ubuntu Desktop differ in both operating environment and configuration. Any observed differences cannot automatically be attributed solely to hardening.