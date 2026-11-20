# Portfolio Roadmap

Each phase remains **not verified** until relevant lab evidence is recorded.

## Phase 1 — Build and inventory
- [x] Capture and identify VMware display names of three VMs: Windows Server 2022, THEPUNISHER, SPIDERMAN (screenshot supplied 2026-11-20).
- [x] Record `HYDRA-DC` guest hostname, DNS suffix and IPv4/DNS configuration from server `ipconfig /all` (2026-11-20).
- [x] Verify HYDRA-DC domain controller identity and Global Catalog via `Get-ADDomainController` (2026-11-20).
- [x] Record THEPUNISHER's IP, DNS, DHCP, gateway and hostname from `ipconfig /all` (2026-11-20).
- [x] Verify THEPUNISHER's domain membership in `MARVEL.local` using `Win32_ComputerSystem` (`PartOfDomain=True`), 2026-11-20.
- [x] Record SPIDERMAN's IP, DNS, DHCP, gateway and hostname from `ipconfig /all` (2026-11-20).
- [x] Verify SPIDERMAN's configured domain membership in `MARVEL.local` using `Win32_ComputerSystem` (`PartOfDomain=True`), 2026-11-20.
- [x] Verify HYDRA-DC VMware Network Adapter is NAT, connected and enabled at power-on (VMware Settings screenshot, 2026-11-20).
- [x] Record user confirmation that THEPUNISHER and SPIDERMAN VMware adapters use NAT (2026-11-20; separate client Settings screenshots not supplied).
- [ ] Assess NAT network containment before intrusive testing; NAT is not fully isolated.
- [ ] Test client DNS resolution and live connectivity to HYDRA-DC.
- [ ] Verify guest operating systems, other VM roles and individual service health.
- [x] Document domain controller and both visible clients (hostnames, addresses, AD membership) from supplied evidence.
- [ ] Verify whether a separate testing VM is used and document its scope if applicable.
- [ ] Produce a network topology diagram using confirmed details.
- [x] Establish at least one successful client-to-server file-sharing connection by direct UNC path (observed during prior lab troubleshooting).

## Phase 2 — Reconnaissance and enumeration
- [ ] Document an authorised discovery and enumeration exercise.
- [ ] Record protocols, exposed services and observations.
- [ ] Explain why each observation matters for AD security.

## Phase 3 — Initial attack-vector assessment
- [ ] Test relevant PEH course techniques safely in the owned lab.
- [ ] Record preconditions, evidence, success/failure and limitations.
- [ ] Describe the associated preventive controls.

## Phase 4 — Post-compromise assessment
- [ ] Document authorised domain enumeration and potential attack paths.
- [ ] Record reproduced credential/privilege issues without publishing secrets.
- [ ] Note how a defender could detect the actions.

## Phase 5 — Reporting and portfolio publication
- [ ] Write findings with impact, proof and remediation.
- [ ] Create a redacted architecture diagram and screenshots.
- [ ] Produce an executive summary and final report.
- [ ] Review files for exposed credentials, internal identifiers and sensitive data.
