# PEH Active Directory Penetration Testing Lab

A hands-on Active Directory security assessment portfolio project based on the **Practical Ethical Hacking (PEH)** course from **TCM Security**, taught by **Heath Adams**.

> **Project status:** In progress. This repository distinguishes verified lab observations from planned exercises. Completing a course video is not proof that an attack or defence was reproduced.

## Overview

This project documents the design, administration and authorised security testing of a small, virtualised Windows Active Directory environment. The aim is to demonstrate practical methodology, evidence-based observations, professional reporting and defensive mitigation.

## Progress snapshot

| Area | Status | Evidence or next action |
| --- | --- | --- |
| Active Directory lab setup | In place; inventory not yet documented | Record actual machines, roles, operating systems and domain settings |
| Windows client-to-server file-sharing connectivity | Verified during lab troubleshooting | Direct UNC access to the server by private IP worked from the client named `Punisher` |
| Network topology diagram | To document | Confirm IPs, virtual networks and machine roles |
| Initial attack vectors | To verify | Attach lab-only observations and redacted screenshots |
| Post-compromise enumeration | To verify | Document commands, authorised scope and results |
| Post-compromise attacks | To verify | Record only exercises personally reproduced |
| Detection, hardening and mitigation | Planned | Map each verified finding to appropriate controls |
| Final penetration testing report | Planned | Summarise scope, methodology, evidence, impact and remediation |

## Repository contents

- [`ROADMAP.md`](ROADMAP.md) — staged portfolio development plan.
- [`docs/01-lab-overview.md`](docs/01-lab-overview.md) — lab purpose, scope and asset inventory.
- [`docs/02-network-architecture.md`](docs/02-network-architecture.md) — known facts and network diagram placeholder.
- [`docs/03-assessment-journal.md`](docs/03-assessment-journal.md) — evidence-based lab log.
- [`docs/04-finding-template.md`](docs/04-finding-template.md) — reusable security finding template.
- [`docs/05-mitigation-mapping.md`](docs/05-mitigation-mapping.md) — planned attack-to-defence notes.
- [`evidence/README.md`](evidence/README.md) — safe evidence-capture and redaction conventions.

## Ethics and scope

All offensive techniques must be performed only against systems owned or expressly authorised for this isolated training lab. No live third-party network testing, credential sharing or distribution of sensitive environment details is included here.

## Documentation standards

For each exercise, capture: **objective, authorised targets, method, observed output, interpretation, mitigation and evidence reference**. Course completion indicators are evidence of course progress, not independent verification of a technical result.

## Next milestone

Confirm the virtual machine inventory and build a network architecture diagram that reflects the actual environment.
