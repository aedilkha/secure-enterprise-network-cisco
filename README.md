# Secure Enterprise Network — Cisco (CCNA-level)

Secure enterprise network design for a **simulated NVIDIA R&D and Production Hub**, built as a team project during a BeCode networking program. The design responds to a full Request for Proposal: 48 workstations across six departments, two floors, a centralized GPU compute node, and a security-first architecture.

> **Note:** "NVIDIA" here is a *simulated client scenario* used for the exercise. This is an educational project and was not produced for or on behalf of NVIDIA Corporation.

---

## Overview

The brief was to design, document, and cost a hierarchical enterprise network for a fictional regional R&D hub, capped at a **€300,000 budget**. The delivered solution centers on a redundant collapsed-core topology with least-privilege segmentation, a DMZ implemented through VLANs and ACLs, and centralized AAA authentication — the whole thing simulated and verified in Cisco Packet Tracer.

## Architecture at a glance

| Element | Choice |
|---|---|
| Topology | Redundant collapsed-core, two-tier hierarchical |
| Core switches | 2× Cisco Catalyst (Catalyst 9300-24T-E in production BOM) |
| Redundancy | HSRPv2 (per-VLAN virtual gateways) + EtherChannel between cores |
| Perimeter firewall | Cisco ASA 5506-X (simulation) / Firepower 1010 (production BOM) |
| Segmentation | 14 VLANs on `10.20.[VLAN].0/24` subnets |
| DMZ | Dedicated VLAN 60, isolated via core ACLs + ASA NAT/ACL policies |
| Internal security | Inbound least-privilege ACLs per department VLAN |
| Authentication | AAA / RADIUS with local fallback, SSHv2 only |
| Services | Split-horizon DNS, centralized DHCP relay, FTP storage, NTP, syslog |
| Compute | Centralized GPU node (4× NVIDIA H100 in BOM) on its own VLAN 75 |
| Budget | €299,028.63 CAPEX — within the €300,000 envelope |

## Key design decisions

- **Collapsed core over a three-tier model** — at 48 workstations (scalable to 144), a dedicated distribution layer would add overhead without benefit.
- **VLAN-based DMZ** — the RFP required a DMZ *through VLANs and ACLs*; isolation is enforced on the core switch SVIs and reinforced by ASA policies.
- **Least-privilege by default** — inter-department traffic is denied unless explicitly permitted; file exchange goes through the FTP server rather than direct VLAN-to-VLAN transfers.
- **Automated ACL generation** — a Python script reads the access-control matrix and outputs paste-ready Cisco IOS, guaranteeing the deployed config matches the documented matrix and always appends a catch-all deny.
- **Scalability to 3×** — addressing and switch sizing allow department growth without a major refactor.

## Repository structure

```
.
├── README.md
├── LICENSE
├── docs/
│   ├── Sharp_Networks_-_NVIDIA_Proposal.pdf   # Full technical proposal
│   ├── NVIDIA_RFQ.pdf                          # Original request (scenario brief)
│   └── NVIDIA_RFP_Scoring_Matrix.pdf           # Evaluation matrix
└── packet-tracer/
    └── sharpnetworks.pkt                       # Simulated & verified network
```

## How to open the simulation

1. Install **Cisco Packet Tracer 9.0 or later**.
2. Open `packet-tracer/sharpnetworks.pkt`.
3. Recommended boot order when reconfiguring from scratch: core switches → services servers (DHCP/NTP/DNS) → services access switch → ASA & DMZ → remaining access switches.

Full paste-ready device configurations are documented in the proposal (Section 14 — Appendices).

## Evaluation results

The project was assessed against a scoring matrix covering technical design, security, operations, budget, and delivery:

| Category | Score (1–5) |
|---|---|
| Security Strategy (DMZ, ACL, segmentation) | 5 |
| Budget & BOM (realism, €300k cap) | 5 |
| Technical Design (modularity, subnetting, VLANs) | 4.5 |
| Operational Plan (DNS, DHCP, AAA, throughput) | 4.5 |
| Professional Pitch | 3.5 |
| Q&A | 3 |

## Tech & skills

Cisco IOS · Catalyst multilayer switching · ASA firewall · HSRPv2 · EtherChannel · VLAN segmentation · extended ACLs · AAA/RADIUS · DHCP relay · split-horizon DNS · NAT/PAT · Packet Tracer · network documentation · Python (ACL automation)

## Team — Sharp Networks

| Member | Role |
|---|---|
| Anton Baudoux De Corte | Project Manager |
| Alvi Edilkhanov | Architecture Manager |
| Jakub Rutyna | Security Engineer |
| Sajjad Shahpoor | Security Engineer |
| Kien Truong | Network Engineer |
| Dji Gapomo | Network Engineer |

## License

Released under the MIT License — see [LICENSE](LICENSE).
