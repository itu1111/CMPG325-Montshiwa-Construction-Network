# IP Addressing Plan : CMPG325-2026-071

**Assigned block:** 10.30.0.0/16 (65,536 addresses)
**Method:** VLSM (Variable Length Subnet Masking), sized to each segment's *assumed*
host requirement (see `client-requirements.md` §3 for how those numbers were reasoned
out) so the block isn't wasted on oversized subnets.

## How the plan was built
1. List every network segment and its assumed host count.
2. Sort segments **largest to smallest** — the golden rule of VLSM; allocating big
   subnets first, on power-of-two boundaries, avoids fragmenting the block.
3. For each segment, find the smallest subnet mask that still covers
   (assumed hosts + 2 for network/broadcast), with a little headroom for growth.
4. Assign the next free, correctly-aligned address block of that size, then move the
   allocation cursor forward.

## Host requirements (from assumptions in client-requirements.md)

| Segment | Site | Assumed hosts | Sized for |
|---|---|---|---|
| Voice VLAN 20 (all HQ depts) | HQ | ~45 | 50 |
| Data VLAN 11 — PM & Engineering | HQ | ~20 | 25 |
| Data VLAN 10 — Admin & Finance | HQ | ~15 | 20 |
| Data VLAN 12 — Procurement & Stores | HQ | ~10 | 12 |
| Data VLAN 30 — Site Office | Branch | 8 | 10 |
| Voice VLAN 40 — Site Office | Branch | up to 10 | 10 |
| WAN link | HQ ↔ ISP | 2 | 2 |
| WAN link | Branch ↔ ISP | 2 | 2 |

## VLSM allocation (largest to smallest)

| Subnet name | VLAN | Network address | Mask (CIDR) | Usable range | Broadcast | Gateway | Usable hosts |
|---|---|---|---|---|---|---|---|
| HQ-VOICE | 20 | 10.30.0.0 | /26 (255.255.255.192) | 10.30.0.1 – 10.30.0.62 | 10.30.0.63 | 10.30.0.1 | 62 |
| HQ-PM-ENGINEERING | 11 | 10.30.0.64 | /27 (255.255.255.224) | 10.30.0.65 – 10.30.0.94 | 10.30.0.95 | 10.30.0.65 | 30 |
| HQ-ADMIN-FINANCE | 10 | 10.30.0.96 | /27 (255.255.255.224) | 10.30.0.97 – 10.30.0.126 | 10.30.0.127 | 10.30.0.97 | 30 |
| HQ-PROCUREMENT | 12 | 10.30.0.128 | /28 (255.255.255.240) | 10.30.0.129 – 10.30.0.142 | 10.30.0.143 | 10.30.0.129 | 14 |
| BRANCH-DATA (Site Office) | 30 | 10.30.0.144 | /28 (255.255.255.240) | 10.30.0.145 – 10.30.0.158 | 10.30.0.159 | 10.30.0.145 | 14 |
| BRANCH-VOICE (Site Office) | 40 | 10.30.0.160 | /28 (255.255.255.240) | 10.30.0.161 – 10.30.0.174 | 10.30.0.175 | 10.30.0.161 | 14 |
| WAN HQ–ISP | — | 10.30.0.176 | /30 (255.255.255.252) | 10.30.0.177 – 10.30.0.178 | 10.30.0.179 | n/a (point-to-point) | 2 |
| WAN Branch–ISP | — | 10.30.0.180 | /30 (255.255.255.252) | 10.30.0.181 – 10.30.0.182 | 10.30.0.183 | n/a (point-to-point) | 2 |

**Point-to-point assignment:**
- 10.30.0.177 = R-HQ Se0/0/0, 10.30.0.178 = R-ISP (HQ-facing interface)
- 10.30.0.181 = R-BR Se0/0/0, 10.30.0.182 = R-ISP (Branch-facing interface)

**Alignment check:** each block starts on a boundary that is a multiple of its own
size (64, 32, 32, 16, 16, 16, 4, 4 addresses respectively) — confirming the VLSM
allocation is valid and non-overlapping.

**Reserved for future growth:** 10.30.0.184 – 10.30.255.255 is left unallocated for
future departments, sites, or VLANs (see assumption in client-requirements.md §3).

## Why this satisfies the design constraint
Every department at HQ gets its **own Data VLAN/subnet**, and both HQ and the branch
have their **data and voice traffic on entirely separate VLANs/subnets** — this is the
addressing-level evidence that VoIP traffic is logically separated from data traffic
(Section 8 of the brief). The Layer 2 configuration that enforces this (trunking,
voice VLAN commands, access ports) will be implemented and evidenced in Milestone 2.
