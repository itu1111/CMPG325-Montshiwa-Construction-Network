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

### Milestone 2 — Client Implementation Review

**Status: Completed**

The Montshiwa Construction network was implemented and tested in Cisco Packet Tracer according to the assigned Default Routing challenge and the Milestone 1 addressing plan.

Completed implementation includes:

* HQ and Branch network topology
* VLAN segmentation for data and voice traffic
* Router-on-a-stick inter-VLAN routing
* HQ and Branch IP addressing using the approved VLSM addressing plan
* HQ–ISP and Branch–ISP serial WAN connections
* Default routing from the HQ and Branch routers through the ISP router
* Static routes on the ISP router
* Cisco IP phones connected using dedicated voice VLANs
* Connectivity testing between HQ and Branch networks

### Testing

End-to-end connectivity was successfully tested between the HQ and Branch networks. Both directions were tested using ICMP ping, with successful responses and 0% packet loss after the network had converged.

### Packet Tracer File

The completed Cisco Packet Tracer implementation is available in the `packet-tracer/` directory.

### Evidence

Screenshots documenting VLAN configuration, routing tables, and successful HQ-to-Branch and Branch-to-HQ connectivity tests are included in the project evidence/screenshots directory.

