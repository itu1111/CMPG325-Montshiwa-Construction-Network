# CMPG325-2026-071 — Montshiwa Construction Network Design

**Student:** Melamu, IGR (51890038) | **Module:** CMPG 325 : Computer Networks (NWU)
**Client:** Montshiwa Construction (Mahikeng) — Client ID CLI-071
**Assigned challenge:** Default Routing (edge/ISP path design) — Intermediate
**Assigned addressing block:** 10.30.0.0/16

## About this project
This repository is my individual portfolio of evidence for the CMPG 325 semester
project: designing, addressing, and (in later milestones) building and testing a
Cisco Packet Tracer network for a construction-industry client, with VoIP/data
traffic separation and a small branch office connected to the main site.

## Repository structure
```
CMPG325-2026-071/
├── README.md                          <- you are here
├── docs/
│   ├── client-requirements.md         <- extracted + interpreted client requirements
│   ├── ip-addressing-plan.md          <- VLSM subnetting plan for 10.30.0.0/16
│   ├── physical-topology.png          <- device/cabling diagram
│   └── logical-topology.png           <- IP/VLAN/routing diagram
├── milestones/
│   └── milestone-1-client-design-review.md
├── packet-tracer/                     <- .pkt file(s) added in later milestones
└── screenshots/                       <- configuration/testing evidence (later milestones)
```


## Scope note
Per module guidance, the project brief is the sole authoritative source for this
project — no contact with the real Montshiwa Construction was made or is required.
Where the brief does not specify a detail (departments, headcounts, services, etc.),
a reasonable, justified network-engineering assumption was made instead. See
`docs/client-requirements.md` §3 for the full assumptions table.

## Academic integrity note
This project was completed by me with AI-assisted tutoring for planning and
explanation, in line with the NWU AI Policy referenced in the project brief.
I remain responsible for the correctness, understanding, and verification of
everything in this repository and in my final Packet Tracer implementation.
