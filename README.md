# Active Directory Enterprise Lab

> Enterprise-style Active Directory lab environment built using Windows Server 2022 to simulate real-world domain infrastructure, DNS/DHCP services, and client integration.

---

## Overview

This repository documents the design and deployment of a small enterprise-style Active Directory environment built in a virtualized lab.

The purpose of this lab is to:

- Strengthen hands-on Windows Server administration skills  
- Reinforce networking fundamentals  
- Simulate production-style infrastructure  
- Build a documented portfolio project aligned with network administration and cybersecurity roles  

---

## Project Objectives

- [x] Deploy Active Directory Domain Services (AD DS)
- [x] Configure DNS for internal name resolution
- [x] Deploy and configure DHCP server role
- [x] Implement structured IP addressing
- [x] Join Windows 11 client to domain
- [ ] Apply advanced Group Policy
- [ ] Implement log forwarding / monitoring
- [ ] Add secondary domain controller

---

## Lab Environment

| Component | Technology |
|------------|------------|
| Hypervisor | VirtualBox |
| Server OS | Windows Server 2022 |
| Client OS | Windows 11 |
| Roles Installed | AD DS, DNS, DHCP |
| Network Type | Internal Network + NAT |

---

## Network Architecture

### Logical Topology

![Network Diagram](diagrams/network-topology.png)

### IP Addressing Plan

| Device | Role | IP Address | Notes |
|--------|------|------------|-------|
| DC01 | Domain Controller | 192.168.100.10 | Static |
| CLIENT01 | Domain Workstation | DHCP | Scope Assigned |
| Scope Range | DHCP Pool | 192.168.100.50–100 | /24 |
| Subnet | Internal Network | 192.168.100.0/24 | 255.255.255.0 |

---

## Technologies Used

- Windows Server 2022  
- Windows 11  
- Active Directory Domain Services  
- DNS Server  
- DHCP Server  
- PowerShell  
- TCP/IP Networking  

---

## Repository Structure

```
Active-Directory-Enterprise-Lab/
│
├── README.md
├── /diagrams
├── /docs
├── /configs
└── /screenshots
```

---

## Implementation Documentation

Detailed walkthroughs are available in the `/docs` folder:

- **01-network-design.md**
- **02-domain-controller-setup.md**
- **03-dns-configuration.md**
- **04-dhcp-configuration.md**
- **05-client-domain-join.md**
- **06-group-policy.md**
- **07-security-hardening.md**
- **08-troubleshooting-notes.md**

---

## Security Considerations

- Static IP assigned to Domain Controller  
- DHCP authorized within Active Directory  
- Internal DNS used for domain resolution  
- Administrative privileges limited to domain accounts  
- Proper DHCP scope options configured (DNS + Domain Name)

---

## Lessons Learned

- DNS configuration must be completed before domain join  
- Incorrect DHCP scope options can prevent domain registration  
- Proper IP planning prevents rework later  
- Structured documentation improves troubleshooting efficiency  

---

## Future Improvements

- Deploy secondary domain controller for redundancy  
- Configure DHCP failover  
- Implement VLAN segmentation  
- Add Windows Event Forwarding  
- Integrate basic SIEM monitoring  

---

## Project Purpose

This lab was built to:

- Reinforce Network+ concepts through hands-on implementation  
- Develop practical Windows Server infrastructure experience  
- Demonstrate enterprise-style system configuration  
- Support transition into network administration and cybersecurity roles  

---

## License

This project is licensed under the MIT License.
