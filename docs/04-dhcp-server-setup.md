# 04 - DHCP Server Setup

## Overview

This document covers the installation and configuration of the DHCP Server role in the `lab.rjhollinger.com` Active Directory home lab environment.

The DHCP server will:

- Automatically assign IP addresses to clients
- Provide DNS configuration (pointing to the Domain Controller)
- Define the usable IP scope for the LAN
- Integrate securely with Active Directory

---

## Lab Environment Details

| Item | Value |
|------|-------|
| Domain Name | lab.rjhollinger.com |
| Domain Controller | HL-DC-01 |
| Server Static IP | 192.168.1.10 |
| Network | 192.168.1.0/24 |
| Default Gateway | 192.168.1.1 |
| DNS Server | 192.168.1.10 |

---

# 1. Install DHCP Server Role

## Using Server Manager

1. Open **Server Manager**
2. Click **Manage → Add Roles and Features**
3. Select:
   - Role-based or feature-based installation
4. Select server: **HL-DC-01**
5. Check:

```
DHCP Server
```

6. Click **Add Features**
7. Complete the installation

---

# 2. Complete Post-Installation Configuration

After installation completes:

1. Click the **notification flag** in Server Manager
2. Select:

```
Complete DHCP configuration
```

3. Use current administrative credentials
4. Authorize the DHCP server in Active Directory

> DHCP must be authorized in AD to issue IP addresses in a domain environment.

---

# 3. Create a DHCP Scope

Open:

```
Server Manager → Tools → DHCP
```

Right-click:

```
IPv4 → New Scope
```

## Scope Configuration

| Setting | Value |
|----------|--------|
| Scope Name | LAB-LAN |
| Start IP | 192.168.1.100 |
| End IP | 192.168.1.200 |
| Subnet Mask | 255.255.255.0 |
| Exclusions | 192.168.1.1 - 192.168.1.50 |
| Lease Duration | Default (8 days) |

---

# 4. Configure Scope Options

During the wizard, configure the following:

## Router (Default Gateway)

```
192.168.1.1
```

## DNS Server

```
192.168.1.10
```

## Parent Domain

```
lab.rjhollinger.com
```

Activate the scope when prompted.

---

# 5. Verify DHCP Service

Open:

```
services.msc
```

Verify:

```
DHCP Server → Running
```

---

# 6. Test Client IP Assignment

On the Windows client VM:

## Release existing IP

```
ipconfig /release
```

## Request new IP

```
ipconfig /renew
```

## Verify configuration

```
ipconfig /all
```

### Expected Results

- IP Address: 192.168.1.100 – 192.168.1.200
- Default Gateway: 192.168.1.1
- DNS Server: 192.168.1.10
- Connection-specific DNS Suffix: lab.rjhollinger.com

---

# Validation Checklist

- [ ] DHCP role installed
- [ ] DHCP authorized in Active Directory
- [ ] Scope created
- [ ] Scope activated
- [ ] Scope options configured
- [ ] Client successfully receives lease

---

# Key DHCP Concepts

| Concept | Explanation |
|----------|-------------|
| Scope | Range of IP addresses available for lease |
| Lease | Temporary assignment of an IP address |
| Authorization | AD security feature preventing rogue DHCP |
| Exclusion Range | IP addresses reserved for infrastructure |
| Scope Options | Network settings distributed to clients |

---

# DHCP DORA Process

When a client joins the network, the following process occurs:

1. **Discover** – Client broadcasts DHCPDISCOVER
2. **Offer** – Server responds with DHCPOFFER
3. **Request** – Client sends DHCPREQUEST
4. **Acknowledge** – Server responds with DHCPACK

---

# Troubleshooting

## Client Not Receiving IP Address

- Ensure scope is activated
- Verify DHCP server is authorized
- Confirm client is on correct VirtualBox network
- Verify firewall rules
- Ensure client DNS points to 192.168.1.10

## DHCP Logs Location

```
C:\Windows\System32\dhcp
```

---

# Security Notes

- Always authorize DHCP in AD environments
- Exclude infrastructure IP ranges
- Consider creating reservations for:
  - Servers
  - Printers
  - Network devices

Security hardening will be covered in:

```
07-security-hardening.md
```

---

# Next Step

Proceed to:

```
05-client-domain-join.md
```

---

# Completion Status

- [ ] Documented
- [ ] Tested
- [ ] Screenshots added
- [ ] Committed to repository
