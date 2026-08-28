# Client Requirements — Montshiwa Construction (Mahikeng)

**Project:** CMPG325-2026-071 | **Client ID:** CLI-071 | **Industry:** Construction

> **Note on scope:** Per module guidance, the project brief is the sole authoritative
> source for this project. No contact with the real organisation was made or is
> required. Where the brief does not specify a detail, a reasonable
> network-engineering assumption has been made below and justified , the goal is a
> technically sound, defensible design for *this scenario*, not a recreation of the
> real Montshiwa Construction's actual network.

## 1. Formal requirements (verbatim from project brief)
| # | Requirement | Source |
|---|---|---|
| R1 | Provide appropriate connectivity and network services for a construction-industry client | Section 6 |
| R2 | Use the assigned addressing block **10.30.0.0/16** as the basis of the IP addressing plan | Section 6 |
| R3 | Implement and demonstrate **Default Routing (edge/ISP path design)** — Intermediate difficulty | Section 9 |
| R4 | **Design constraint:** VoIP traffic must be logically separated from data traffic | Section 8 |
| R5 | **Change request CR6:** a small branch office of 6–10 users must be connected to the main (head office) network | Section 10 |
| R6 | Solution must be built and simulated in Cisco Packet Tracer and produce a working, testable `.pkt` file | Sections 1, 7, 15 |
| R7 | All design decisions, evidence, and testing must be documented in a GitHub portfolio | Section 12 |

## 2. Scenario analysis : reasoning about the client
The brief only states the client is a **construction company based in Mahikeng**.
Reasoning from that alone, about what such a company's head office would realistically
need to run day-to-day:

- Construction firms manage **quotes, invoices and payroll** → an **Administration &
  Finance** function.
- They run **active building/construction projects** that need engineers, quantity
  surveyors, and project managers coordinating from the head office → a **Project
  Management & Engineering** function.
- They order and track **materials, equipment and suppliers** → a **Procurement &
  Stores** function.
- Everyone above needs a desk phone as well as a PC → **VoIP is used company-wide**,
  which is exactly why the brief's constraint (VoIP separated from data) matters.

This reasoning is the basis for the three head-office departments modelled in the
topology and addressing plan, rather than a single undifferentiated "office LAN."

## 3. Assumptions made (documented, per module guidance)
No client contact was made; the following assumptions fill the gaps the brief leaves
open, and are what will be defended in the video demonstration:

| Assumption | Value chosen | Justification |
|---|---|---|
| Head-office departments | Admin & Finance, Project Management & Engineering, Procurement & Stores | Minimum functional split for a small-to-mid construction company; keeps the topology realistic without over-engineering it |
| Admin & Finance headcount | ~15 users | Small back-office team is typical for a firm this size |
| PM & Engineering headcount | ~20 users | Largest department — most staff at a construction HQ coordinate active projects |
| Procurement & Stores headcount | ~10 users | Smaller support function |
| HQ IP phones | One shared Voice VLAN sized for ~45–50 phones (all departments) | VoIP is simplest to manage as a single voice VLAN spanning departments rather than per-department voice VLANs, since call routing/QoS policy is usually company-wide, not per-department |
| Branch/site office size | 8 data users + up to 10 phones | Falls within CR6's stated 6–10 user range; a couple of spare voice ports are budgeted in case supervisors also get phones |
| Branch office role | A construction site office (site supervisors, site clerk, resident engineer) | Consistent with "branch office" in a construction context — a site office is the standard small remote location for this industry, not a retail branch |
| WAN connectivity type | Serial point-to-point links from each site's router to a shared ISP/edge router | Matches the assigned "Default Routing (edge/ISP path design)" challenge directly — both sites are single-homed stub networks |
| Address space left unused above 10.30.0.184 | Reserved, unallocated | Realistic ISP-block practice: leave room for future sites/departments rather than consuming the whole /16 immediately |

## 4. Design requirements derived from the above
- **Traffic separation (R4):** implemented with per-department **Data VLANs** (VLAN 10,
  11, 12) plus a company-wide **Voice VLAN** (VLAN 20) at HQ, and an equivalent
  Data/Voice VLAN pair (VLAN 30/40) at the branch — VLANs are the standard, scalable
  way to achieve logical traffic separation on switched Cisco infrastructure without
  needing separate physical cabling.
- **Default routing (R3):** both HQ and Branch routers are single-homed edges (one path
  out, to the ISP), so each carries one static default route (`ip route 0.0.0.0
  0.0.0.0 <next-hop>`) rather than a dynamic routing protocol. The ISP/edge router
  holds static routes back to each site's VLANs so the two sites can reach each other
  and simulate reaching the wider internet through it.
- **Addressing (R2):** every subnet is carved from 10.30.0.0/16 using VLSM, sized to
  the assumed headcounts above rather than arbitrarily (see `ip-addressing-plan.md`).
- **CR6 (R5):** the branch/site office is modelled as its own router + switch + VLAN
  pair, connected back to the main network via the shared ISP edge router.

## 5. Out of scope for Milestone 1
Router/switch configuration, VLAN trunking commands, DHCP, and testing evidence are
covered in Milestone 2 and the final submission — this milestone covers scenario
analysis, requirements, topology, and addressing only.
