# Cisco Packet Tracer — Commands Cheatsheet

Quick reference for all important CLI commands used in these labs.

---

## Navigation

| Command | What it does |
|---------|--------------|
| `en` | Enter privileged mode (Switch> → Switch#) |
| `conf t` | Enter global configuration mode (Switch# → Switch(config)#) |
| `exit` | Go back one level |
| `end` | Jump straight back to Switch# from anywhere |
| `Ctrl+C` | Cancel current operation |
| `Ctrl+Shift+6` | Escape stuck operations (e.g. DNS lookup) |
| `?` | Show all available commands in current mode |

---

## VLAN Commands (Switch)

| Command | What it does |
|---------|--------------|
| `vlan 10` | Create VLAN 10 (inside conf t) |
| `name IT` | Name the VLAN (inside vlan config) |
| `no vlan 10` | Delete VLAN 10 |
| `show vlan brief` | Show all VLANs and their assigned ports |

---

## Port Assignment (Switch)

| Command | What it does |
|---------|--------------|
| `interface fa0/1` | Enter interface FastEthernet 0/1 |
| `interface gig0/1` | Enter interface GigabitEthernet 0/1 |
| `switchport mode access` | Set port to access mode (for end devices) |
| `switchport access vlan 10` | Assign port to VLAN 10 |
| `switchport mode trunk` | Set port to trunk mode (carries all VLANs) |
| `show interfaces trunk` | Show all trunk ports and which VLANs they carry |
| `show interfaces gig0/1 switchport` | Show detailed switchport info for a specific port |

---

## Router-on-a-Stick (Router)

| Command | What it does |
|---------|--------------|
| `interface gig0/1` | Enter physical interface |
| `no shutdown` | Bring interface up (interfaces are down by default) |
| `interface gig0/1.10` | Create subinterface for VLAN 10 |
| `encapsulation dot1q 10` | Tag subinterface with VLAN 10 (802.1Q) |
| `ip address 192.168.10.1 255.255.255.0` | Assign IP — this becomes the gateway for VLAN 10 |
| `no interface gig0/1.10` | Delete a subinterface |

---

## ACL — Access Control Lists (Router)

| Command | What it does |
|---------|--------------|
| `access-list 100 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255` | Block Guest subnet from reaching IT subnet |
| `access-list 100 permit ip any any` | Allow all other traffic |
| `ip access-group 100 in` | Apply ACL 100 inbound on an interface |
| `no access-list 100` | Delete ACL 100 completely |
| `show access-lists` | Show all configured ACLs |

**Wildcard mask explained:**
- `0.0.0.255` = entire /24 subnet (opposite of 255.255.255.0)
- `0` = must match exactly
- `255` = any value allowed

---

## Verification — Show Commands

| Command | Where | What it shows |
|---------|-------|---------------|
| `show vlan brief` | Switch | All VLANs and assigned ports |
| `show interfaces trunk` | Switch | Trunk ports and VLANs they carry |
| `show ip interface brief` | Router | All interfaces, IP addresses, and up/down status |
| `show ip route` | Router | Routing table — all known networks |
| `show access-lists` | Router | All configured ACLs and hit counts |
| `show running-config` | Router/Switch | Full current configuration |

---

## PC Commands (Command Prompt)

| Command | What it does |
|---------|--------------|
| `ping 192.168.10.2` | Test connectivity to an IP |
| `ipconfig` | Show PC's current IP, subnet, and gateway |

---

## Common Mistakes & Fixes

| Problem | Fix |
|---------|-----|
| `conf t` says invalid | Type `end` first to get back to Switch# |
| Stuck on DNS lookup | Press `Ctrl+Shift+6` to escape |
| Cable is red/orange | Interface is down — use `no shutdown` on router |
| Inter-VLAN ping fails | Check trunk port on switch, check subinterfaces on router |
| `% Access VLAN does not exist` | Normal warning — VLAN gets created automatically |
| `% overlaps with another interface` | Same subnet assigned to two interfaces — change the IP |

---

