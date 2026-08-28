# Milestone 1 : Client Design Review

**Project:** CMPG325-2026-071 (Montshiwa Construction, Mahikeng)
**Due:** 28 August 2026

## Deliverables in this milestone
1. [Client Requirements](../docs/client-requirements.md) : includes scenario analysis
   and a documented, justified assumptions table (per module guidance that the brief
   is the sole authoritative source)
2. [Physical Topology](../docs/physical-topology.png)
3. [Logical Topology](../docs/logical-topology.png)
4. [IP Addressing Plan](../docs/ip-addressing-plan.md)
5. This GitHub repository itself (initial commit)

## Summary of design decisions
- **Two-site topology:** Head Office (three departments: Admin & Finance, Project
  Management & Engineering, Procurement & Stores) and a CR6 branch/site office,
  both single-homed to a shared ISP/edge router, the natural fit for "Default
  Routing (edge/ISP path design)."
- **Per-department Data VLANs (10, 11, 12)** at HQ plus a company-wide **Voice VLAN
  (20)**, and an equivalent **Data/Voice VLAN pair (30/40)** at the branch, to satisfy
  the VoIP/data separation constraint realistically rather than with a single flat LAN.
- **10.30.0.0/16** subnetted with VLSM into one /26, two /27s, three /28s and two /30s,
  sized to the assumed department headcounts, with the remainder of the block
  reserved for growth.
- All assumptions (department structure, headcounts, branch role, WAN link type) are
  documented and justified in `client-requirements.md`, since no contact with the real
  organisation was made or required.

## Not yet done (planned for Milestone 2 / final submission)
- Building the topology in Cisco Packet Tracer
- Interface, VLAN, trunk, and default-route configuration
- DHCP (if used) for phones/PCs
- Connectivity testing and screenshot evidence
- Troubleshooting log and final reflection
