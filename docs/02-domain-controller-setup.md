# 02 – Domain Controller Setup

> Project: Active Directory Home Lab
> Role: Domain Controller (HL-DC-01)
> OS: Windows Server 2022 
> Author: Robert Hollinger 

---

## Overview

This document outlines the full process of deploying and configuring a Windows Server machine as an Active Directory Domain Controller in a home lab environment.

The goal of this lab is to:

- Deploy a functional Active Directory Domain Services (AD DS) environment
- Configure DNS properly for domain functionality
- Promote the server to a Domain Controller
- Validate authentication and domain operations
- Simulate a real-world small enterprise setup

---

## Why This Matters

Active Directory is foundational in enterprise environments.  
Understanding how to deploy and configure a Domain Controller is critical for:

- Network Administration roles
- Cybersecurity / SOC environments
- Identity & Access Management (IAM)
- Group Policy management
- Windows enterprise environments

This lab demonstrates hands-on experience with:

- AD DS deployment
- DNS configuration
- Server role installation
- Domain promotion
- Post-install validation

---

## Lab Environment

| Component | Value |
|-----------|-------|
| Hypervisor | VirtualBox |
| Server Name | HL-DC-01 |
| Domain Name | lab.rjhollinger.com |
| IP Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Gateway | 192.168.1.1 |
| DNS | 127.0.0.1 (after promotion) |

---

## Network Configuration

### Step 1 – Configure Static IP

Before installing Active Directory, configure a static IP address.

**Why?**  
Domain Controllers must not rely on DHCP for addressing.

Settings used:

- IP: `192.168.1.10`
- Subnet: `255.255.255.0`
- Gateway: `192.168.1.1`
- Preferred DNS: `127.0.0.1` (after AD install)

> Important: Do NOT leave DNS pointing to your router once AD is installed.

---

## Install Active Directory Domain Services (AD DS)

### Step 2 – Add Server Role

1. Open **Server Manager**
2. Click **Add Roles and Features**
3. Select:
   - Active Directory Domain Services
4. Include required features
5. Install

You should now see a notification to **Promote this server to a domain controller**.

---

## Promote to Domain Controller

### Step 3 – Create a New Forest

1. Click **Promote this server to a domain controller**
2. Select:
   - “Add a new forest”
3. Root domain name:
   lab.rjhollinger.com

### Step 4 – Domain Controller Options

- Forest Functional Level: Windows Server 2022
- Domain Functional Level: Windows Server 2022
- DNS Server: Checked
- Global Catalog: Checked
- DSRM Password: (Set secure password)

---

## Server Reboot

After promotion:

- Server will automatically reboot
- Login using:
  LAB\Administrator

---

## Post-Install Validation

### Verify AD Installation

Open:

- Active Directory Users and Computers
- DNS Manager
- Active Directory Domains and Trusts

Confirm:

- Domain appears correctly
- DNS forward lookup zone created
- SYSVOL and NETLOGON shares exist

Run in PowerShell:

```powershell
dcdiag
```

Confirm there are no critical errors.

---

## Testing Domain Functionality

### Test 1 - Join a Client Machine

1. Set client DNS to:
```Code
192.168.1.10
```

2. Join domain:
```Code
lab.local
```

3. Reboot client
4. Login with domain credentials

### Test 2 - Create Test User
- Create user in ADUC
- Add to appropriate OU
- Test login from client

---

## Security Considerations
- Disable unused services
- Rename default Administrator account (optional lab hardening)
- Configure basic Group Policy
- Ensure firewall is properly configured
- Avoid exposing DC to internet

## Future Improvements
- Add secondary Domain Controller
- Implement DHCP role on separate server
- Configure Organizational Units properly
- Implement Group Policy baselines
- Simulate privilege escalation scenarios for SOC testing

## Summary
This document walked through:
- Installing AD DS
- Promoting a server to a Domain Controller
- Configuring DNS
- Validating domain functionality
- Testing client authentication

This lab simulates a real-world enterprise identity environment and builds foundational knowledge required for Network Administration and Cybersecurity roles.
