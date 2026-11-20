# PEH Active Directory Penetration Testing Lab

A hands-on Active Directory security assessment portfolio project based on the **Practical Ethical Hacking (PEH)** course from **TCM Security**, taught by **Heath Adams**.

> **Project status:** In progress. This repository distinguishes verified lab observations from planned exercises. Completing a course video is not proof that an attack or defence was reproduced.

## Overview

This project documents the design, administration and authorised security testing of a small, virtualised Windows Active Directory environment. The aim is to demonstrate practical methodology, evidence-based observations, professional reporting and defensive mitigation.

## Progress snapshot

| Area | Status | Evidence or next action |
| --- | --- | --- |
| Active Directory lab inventory | Windows hosts documented; Kali interface captured | HYDRA-DC (.128), SPIDERMAN (.129), THEPUNISHER (.130) in MARVEL.local; Kali Linux eth0 (.131/24). Kali NAT adapter screenshot-verified; VMware VMnet8 NAT subnet identified as 192.168.158.0; connectivity and containment still to verify |
| Windows client-to-server file-sharing connectivity | Previously observed in troubleshooting | Direct UNC access by private IP worked; the earlier address differs from the current server IP and needs reconciliation |
| Network topology diagram | Documented (Mermaid) | All four VMs are configured for NAT; VMnet8 is identified as NAT subnet 192.168.158.0; Windows clients reached HYDRA-DC TCP/389 |
| DNS, LDAP transport and DC discovery | Verified for tested functions | THEPUNISHER resolved HYDRA-DC; both clients reached TCP 389; SPIDERMAN successfully located HYDRA-DC with `nltest /dsgetdc`; subsequent `Test-ComputerSecureChannel` returned **False**; LDAP authentication not yet tested |
| SPIDERMAN secure-channel diagnostics | Mixed results — investigation pending | `Test-ComputerSecureChannel` remained `False` when repeated elevated (user-reported); `nltest /sc_query` previously showed last-used `NERR_Success`. Unresolved, deferred; see [issue #1](https://github.com/ObitoX86/PEH-active-directory-lab/issues/1) |
| Active Directory OU and group inventory | Partially documented | Two OUs identified; 20 groups found directly under `Groups`; `Domain Admins` has 3 verified direct user members (`Administrator`, `Tony Stark`, `SQL Service`). Other memberships and GPOs pending |
| Intentional service-account attack setup | Prerequisite documented | `SQLService` is a direct member of `Domain Admins` and has SPN `HYDRA-DC/SQLService.MARVEL.local:60111`, deliberately configured for the PEH lab. No Kerberoasting execution verified |
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
- [`docs/06-active-directory-structure.md`](docs/06-active-directory-structure.md) — OU inventory and directory-structure evidence.
- [`evidence/README.md`](evidence/README.md) — safe evidence-capture and redaction conventions.
- [`evidence/INDEX.md`](evidence/INDEX.md) — verified index of all 14 uploaded, redacted lab screenshots, each linked to its corresponding PNG.

## Screenshot evidence

**14 of 14 lab setup and verification screenshots are in GitHub.** Selected evidence is displayed below. The [complete evidence gallery](evidence/INDEX.md) displays all 14 images directly, and the [network architecture document](docs/02-network-architecture.md) embeds them beside the corresponding test results.

### Virtual machines

![VMware Workstation inventory](evidence/2026-11-20_01-vmware-inventory.png)

### Domain controller verification

![HYDRA-DC Active Directory identity](evidence/2026-11-20_03-hydra-dc-ad-identity.png)

### Client connectivity verification

![SPIDERMAN TCP 389 test](evidence/2026-11-20_11-spiderman-ldap-tcp389.png)

### Outstanding secure-channel check

![SPIDERMAN secure-channel check reported False](evidence/2026-11-20_13-spiderman-secure-channel-failure.png)

## Ethics and scope

All offensive techniques must be performed only against systems owned or expressly authorised for this controlled virtual lab. NAT networking is not the same as a fully isolated network. No live third-party network testing, credential sharing or distribution of sensitive environment details is included here.

## Documentation standards

For each exercise, capture: **objective, authorised targets, method, observed output, interpretation, mitigation and evidence reference**. Course completion indicators are evidence of course progress, not independent verification of a technical result.

## Next milestone

VMnet8 NAT is identified; all four baseline snapshots (Kali, HYDRA-DC, THEPUNISHER, SPIDERMAN) are user-confirmed. All four recovery-point names have been checked in VMware Snapshot Manager (user-confirmed). The dedicated LAN segment `PEH-AD-Isolated` is user-confirmed as created. HYDRA-DC was user-reported moved there, and a fresh `ipconfig /all` screenshot confirms its static 192.168.158.128/24, no gateway, and loopback DNS settings were preserved. Kali is now shown configured for the isolated LAN segment with post-boot `eth0` at 192.168.158.131/24 and no IPv4 default route (screenshot-verified). ICMP from Kali to HYDRA-DC is now verified (4/4 successful pings, 0% loss). THEPUNISHER was user-reported moved to `PEH-AD-Isolated`; a new post-boot screenshot confirms static `192.168.158.130/24`, no gateway, and DNS `192.168.158.128` remain intact. Its TCP connectivity to HYDRA-DC is now verified again on the isolated LAN (`TcpTestSucceeded=True`, source .130, destination .128:389). SPIDERMAN is the remaining Windows client to migrate. The deliberately privileged, SPN-bearing `SQLService` account is documented as a lab prerequisite, not as a completed compromise. SPIDERMAN's unresolved secure-channel result is tracked in [issue #1](https://github.com/ObitoX86/PEH-active-directory-lab/issues/1) and will be revisited if an exercise depends on it.
