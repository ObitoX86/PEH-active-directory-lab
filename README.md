# PEH Active Directory Penetration Testing Lab

A hands-on Active Directory security assessment portfolio project based on the **Practical Ethical Hacking (PEH)** course from **TCM Security**, taught by **Heath Adams**.

> **Project status:** In progress. This repository distinguishes verified lab observations from planned exercises. Completing a course video is not proof that an attack or defence was reproduced.

## Overview

This project documents the design, administration and authorised security testing of a small, virtualised Windows Active Directory environment. The aim is to demonstrate practical methodology, evidence-based observations, professional reporting and defensive mitigation.

## Progress snapshot

| Area | Status | Evidence or next action |
| --- | --- | --- |
| Active Directory lab inventory | Documented from supplied evidence | HYDRA-DC (192.168.158.128), SPIDERMAN (.129), THEPUNISHER (.130), all in MARVEL.local; client membership confirmed |
| Windows client-to-server file-sharing connectivity | Previously observed in troubleshooting | Direct UNC access by private IP worked; the earlier address differs from the current server IP and needs reconciliation |
| Network topology diagram | Documented (Mermaid) | All three machines use NAT (server screenshot, clients user-confirmed); the two clients each reached HYDRA-DC TCP/389 |
| DNS, LDAP transport and DC discovery | Verified for tested functions | THEPUNISHER resolved HYDRA-DC; both clients reached TCP 389; SPIDERMAN successfully located HYDRA-DC with `nltest /dsgetdc`; subsequent `Test-ComputerSecureChannel` returned **False**; LDAP authentication not yet tested |
| SPIDERMAN secure-channel diagnostics | Mixed results — investigation pending | Direct `Test-ComputerSecureChannel` returned `False`; `nltest /sc_query` reported last-used status `NERR_Success`. No repair performed; rerun direct test elevated |
| Initial attack vectors | To verify | Attach lab-only observations and redacted screenshots |
| Post-compromise enumeration | To verify | Document commands, authorised scope and results |
| Post-compromise attacks | To verify | Record only exercises personally reproduced |
| Detection, hardening and mitigation | Planned | Map each verified finding to appropriate controls |
| Final penetration testing report | Planned | Summarise scope, methodology, evidence, impact and remediation |

## Repository contents

- [`ROADMAP.md`](ROADMAP.md) — staged portfolio development plan.
- [`docs/01-lab-overview.md`](docs/01-lab-overview.md) — lab purpose, scope and asset inventory.
- [`docs/02-network-architecture.md`](docs/02-network-architecture.md) — IP configurations, validated client TCP paths and Mermaid topology.
- [`docs/03-assessment-journal.md`](docs/03-assessment-journal.md) — evidence-based lab log.
- [`docs/04-finding-template.md`](docs/04-finding-template.md) — reusable security finding template.
- [`docs/05-mitigation-mapping.md`](docs/05-mitigation-mapping.md) — planned attack-to-defence notes.
- [`evidence/README.md`](evidence/README.md) — safe evidence-capture and redaction conventions.

## Ethics and scope

All offensive techniques must be performed only against systems owned or expressly authorised for this controlled virtual lab. NAT networking is not the same as a fully isolated network. No live third-party network testing, credential sharing or distribution of sensitive environment details is included here.

## Documentation standards

For each exercise, capture: **objective, authorised targets, method, observed output, interpretation, mitigation and evidence reference**. Course completion indicators are evidence of course progress, not independent verification of a technical result.

## Next milestone

Rerun SPIDERMAN's direct secure-channel test in elevated PowerShell to investigate the disagreement between `Test-ComputerSecureChannel` and the last-used state reported by `nltest /sc_query`, without repairing or resetting anything.
