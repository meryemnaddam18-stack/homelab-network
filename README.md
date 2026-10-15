# Enterprise Network Homelab (Cisco Packet Tracer)

## Overview
Small company network: HQ with segmented VLANs, a branch office
connected over OSPF, and internet access through NAT.

## Topology
![Topology](topology/topology.png)

## Technologies
VLANs · 802.1Q trunking · Router-on-a-stick · DHCP · OSPFv2 · PAT · ACLs · SSH

## Devices

| Device | Model | Role |
|---|---|---|
| HQ-R1 | ISR 4331 | HQ router: inter-VLAN routing, DHCP, NAT, ACLs, OSPF |
| BR-R1 | ISR 4331 | Branch router: OSPF, DHCP |
| ISP | ISR 4331 | Simulated internet provider |
| HQ-SW1 | 2960-24TT | HQ access switch |
| BR-SW1 | 2960-24TT | Branch access switch |
| PCs | PC-PT | Staff-PC1, Staff-PC2, Guest-PC1, Admin-PC, Branch-PC1 |

OSPF runs between HQ and the branch only. HQ-R1 has a default route to the
ISP and advertises it to the branch through OSPF, as a real ISP would not
run an IGP with its customer.

## Addressing Plan

### VLANs

| VLAN | Name | Switch | Purpose |
|---|---|---|---|
| 10 | STAFF | HQ-SW1 | Employee PCs |
| 20 | GUEST | HQ-SW1 | Visitors (internet only) |
| 30 | BRANCH-USERS | BR-SW1 | Branch PCs and switch management |
| 99 | MGMT | HQ-SW1 | Switch management and Admin-PC |
| 998 | PARKING | Both | Unused ports (shut down) |
| 999 | NATIVE | HQ-SW1 | Native VLAN on the trunk (no data) |

### IP addressing

| Network | Subnet | Gateway | DHCP range |
|---|---|---|---|
| VLAN 10 Staff | 192.168.10.0/24 | .1 (HQ-R1 G0/0/0.10) | .11 – .254 |
| VLAN 20 Guest | 192.168.20.0/24 | .1 (HQ-R1 G0/0/0.20) | .11 – .254 |
| VLAN 99 Mgmt | 192.168.99.0/24 | .1 (HQ-R1 G0/0/0.99) | static only |
| VLAN 30 Branch | 192.168.30.0/24 | .1 (BR-R1 G0/0/0) | .11 – .254 |
| HQ ↔ Branch link | 10.0.0.0/30 | HQ .1 / BR .2 | — |
| HQ ↔ ISP link | 203.0.113.0/30 | ISP .1 / HQ .2 | — |
| "Internet" | 8.8.8.8/32 | ISP Loopback0 | — |

### Static addresses

| Device | Interface | Address |
|---|---|---|
| HQ-SW1 | Vlan99 | 192.168.99.2 |
| Admin-PC | NIC | 192.168.99.10 |
| BR-SW1 | Vlan30 | 192.168.30.2 |

Addresses .2 to .10 in each subnet are reserved for devices with fixed IPs,
so DHCP starts at .11.

### Port and cabling map

| From | Port | To | Port | Type |
|---|---|---|---|---|
| HQ-R1 | G0/0/0 | HQ-SW1 | G0/1 | 802.1Q trunk (native 999) |
| HQ-R1 | G0/0/1 | BR-R1 | G0/0/1 | Routed link |
| HQ-R1 | G0/0/2 | ISP | G0/0/0 | Routed link (outside) |
| BR-R1 | G0/0/0 | BR-SW1 | G0/1 | Access VLAN 30 |
| HQ-SW1 | Fa0/1–2 | Staff-PC1, Staff-PC2 | — | Access VLAN 10 |
| HQ-SW1 | Fa0/3 | Guest-PC1 | — | Access VLAN 20 |
| HQ-SW1 | Fa0/4 | Admin-PC | — | Access VLAN 99 |
| BR-SW1 | Fa0/1 | Branch-PC1 | — | Access VLAN 30 |

All other switch ports are in VLAN 998 and shut down.

## Security Policy

1. Guest (VLAN 20) **cannot** reach Staff, Management, or the Branch. Internet access only.
2. Only Admin-PC (VLAN 99) can SSH into routers and switches.
3. Staff and Branch can reach each other and the internet.

### Hardening applied
- SSH version 2 only (2048-bit RSA keys), Telnet disabled on all VTY lines
- Local user authentication, hashed `enable secret`, idle timeouts
- Native VLAN moved off VLAN 1, DTP disabled on the trunk, allowed VLANs restricted
- Unused switch ports parked in VLAN 998 and shut down; VLAN 1 interface disabled

> Passwords in configs are lab examples. In production, secrets would never
> be committed to a repository.

## Build Phases
- [x] Phase 0 – Planning
- [x] Phase 1 – Base config & SSH
- [x] Phase 2 – VLANs & trunking
- [ ] Phase 3 – Inter-VLAN routing
- [ ] Phase 4 – DHCP
- [ ] Phase 5 – OSPF
- [ ] Phase 6 – NAT
- [ ] Phase 7 – ACLs

## Repository Structure

| Path | Contents |
|---|---|
| `topology/` | Packet Tracer file (`homelab.pkt`) and network diagram |
| `configs/` | Running configuration of each device |
| `verification/` | `show` command outputs and tests proving each phase works |
| `troubleshooting.md` | Problems encountered, their cause and fix |

## Notes
- The Catalyst 2960 image in Packet Tracer does not support
  `security passwords min-length` or `terminal length`; 10+ character
  passwords are enforced manually on the switches.
- See [troubleshooting.md](troubleshooting.md) for issues met during the build.
