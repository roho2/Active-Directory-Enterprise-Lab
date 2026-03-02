# 03 - DNS Configuration

## Overview

This document outlines the configuration of DNS for the Active Directory lab environment.

DNS is a **critical dependency** for Active Directory. Without properly configured DNS:

- Clients cannot locate the Domain Controller
- Domain joins will fail
- Group Policy will not apply
- Kerberos authentication will break

In this lab, DNS is hosted on:

| Item | Value |
|------|-------|
| Domain Name | lab.rjhollinger.com |
| DNS Server | HL-DC-01 |
| DNS Server IP | 192.168.1.10 |
| Network | 192.168.1.0/24 |

---

## Why DNS is Critical for Active Directory

Active Directory relies on DNS for:

- Locating Domain Controllers via SRV records
- Kerberos authentication
- Global Catalog discovery
- Domain services resolution

When a client attempts to join `lab.rjhollinger.com`, it queries DNS for:

```
_ldap._tcp.dc._msdcs.lab.rjhollinger.com
```

If DNS is not functioning correctly, the domain join will fail.

---

## DNS Installation

DNS was installed automatically during the Active Directory Domain Services (AD DS) role installation on:

**Server Name:** HL-DC-01  
**IP Address:** 192.168.1.10  

To verify DNS role installation:

1. Open **Server Manager**
2. Click **Tools**
3. Select **DNS**

---

## Forward Lookup Zone

After promoting the server to Domain Controller, a Forward Lookup Zone was automatically created:

```
lab.rjhollinger.com
```

### To Verify the Zone:

1. Open **DNS Manager**
2. Expand **HL-DC-01**
3. Expand **Forward Lookup Zones**
4. Confirm the existence of:

```
lab.rjhollinger.com
```

### Records That Should Exist

Within the zone, you should see:

- Host (A) record for HL-DC-01
- NS record
- SOA record
- _msdcs folder
- SRV records under:
  - _tcp
  - _udp

These SRV records are essential for domain functionality.

---

## Reverse Lookup Zone Configuration

Reverse DNS allows IP addresses to resolve back to hostnames.

### Create Reverse Lookup Zone

1. In DNS Manager, right-click **Reverse Lookup Zones**
2. Select **New Zone**
3. Choose:
   - Primary Zone
   - Store in Active Directory
   - Replicate to all DNS servers in the domain
4. Network ID:

```
192.168.1
```

5. Allow only secure dynamic updates

After creation, verify a PTR record exists for:

```
192.168.1.10 → HL-DC-01.lab.rjhollinger.com
```

---

## DNS Server Settings

### Set Forwarders (Optional but Recommended)

Forwarders allow DNS queries for external domains (like google.com) to resolve.

1. Right-click **HL-DC-01**
2. Select **Properties**
3. Go to **Forwarders** tab
4. Add:
   - 8.8.8.8
   - 1.1.1.1

This allows internal clients to resolve external domains.

---

## 💻 Client DNS Configuration

For domain functionality, clients MUST use:

```
Preferred DNS Server: 192.168.1.10
```

Clients should **NOT** use:
- ISP DNS
- Public DNS (8.8.8.8 directly)

### Windows 11 Client Configuration

1. Open Network Settings
2. Edit adapter settings
3. Select Internal Network adapter
4. Set DNS manually to:

```
192.168.1.10
```

---

## Testing DNS Functionality

### Test 1: Name Resolution

From client machine:

```
nslookup HL-DC-01
```

Expected result:

```
Name: HL-DC-01.lab.rjhollinger.com
Address: 192.168.1.10
```

---

### Test 2: Domain Resolution

```
nslookup lab.rjhollinger.com
```

---

### Test 3: SRV Record Lookup

```
nslookup
set type=SRV
_ldap._tcp.dc._msdcs.lab.rjhollinger.com
```

If this resolves correctly, Active Directory DNS is functioning properly.

---

## Security Considerations

- Allow only secure dynamic updates
- Disable zone transfers unless required
- Do not expose internal DNS to public internet
- Regularly monitor DNS logs for anomalies

DNS is frequently targeted in attacks such as:
- DNS poisoning
- Tunneling
- Amplification attacks

Proper monitoring is critical in production environments.

---

## Troubleshooting Tips

| Issue | Possible Cause |
|-------|----------------|
| Client cannot join domain | Wrong DNS server configured |
| nslookup fails | DNS service not running |
| No SRV records | AD not properly registered |
| Internet doesn't resolve | Forwarders not configured |

### Restart DNS Service (If Needed)

```
services.msc
```

Restart:

```
DNS Server
```

---

## Validation Checklist

- [ ] Forward Lookup Zone exists
- [ ] Reverse Lookup Zone exists
- [ ] SRV records present
- [ ] Client DNS set to 192.168.1.10
- [ ] nslookup successful
- [ ] External resolution works (if forwarders configured)

---

## Related Documents

- 01-network-design.md
- 02-domain-controller-setup.md
- 04-dhcp-server-setup.md
- 05-client-domain-join.md

---

## Key Takeaways

- DNS is the backbone of Active Directory
- Clients must point ONLY to the Domain Controller for DNS
- SRV records are essential for domain operations
- Always verify with nslookup before troubleshooting deeper issues

---

End of Document
