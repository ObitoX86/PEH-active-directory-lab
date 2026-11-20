# Lab Overview and Scope

## Objective

Practise Windows Active Directory configuration, security assessment and mitigation in an isolated environment as part of TCM Security's PEH course, and document evidence for a professional portfolio.

## Confirmed context

- A VMware Workstation inventory screenshot supplied on **2026-11-20** shows three VM entries: **Windows Server 2022**, **THEPUNISHER**, and **SPIDERMAN**. These are VMware display names, not independent proof of the installed operating systems or domain roles.
- During earlier lab troubleshooting, a Windows machine referred to as **Punisher** successfully accessed the lab Windows server through a direct UNC network path by private IP. The relationship between the earlier name and **THEPUNISHER** in the inventory remains to be confirmed.
- VMware inventory screenshot, server `ipconfig /all` screenshot and `Get-ADDomainController` screenshot are available in the chat as evidence; redacted copies have not yet been committed to the repository.
- The server's Windows configuration reports **host name `HYDRA-DC`** and **primary DNS suffix `MARVEL.local`**. The Active Directory PowerShell command `Get-ADDomainController -Identity HYDRA-DC` successfully returned `HYDRA-DC.MARVEL.local`, domain `MARVEL.local`, IPv4 `192.168.158.128`, and `IsGlobalCatalog=True` (2026-11-20). This verifies the directory identifies the machine as a domain controller and as a Global Catalog; individual services and client connectivity remain to be tested.

These observations do **not** establish administrator access or success of any penetration testing technique.

## VM inventory — based on VMware Workstation screenshot

| VMware display name | Expected purpose — not yet verified | Installed OS / domain role | Network settings / IP | Evidence |
| --- | --- | --- | --- | --- |
| Windows Server 2022 | Domain controller (verified by AD PowerShell query) | Guest hostname `HYDRA-DC`; directory lists it as a Global Catalog | IPv4 `192.168.158.128/24`; DHCP disabled; no default gateway; DNS `::1`, `127.0.0.1` | VMware inventory, `ipconfig /all` and `Get-ADDomainController` screenshots, 2026-11-20 |
| THEPUNISHER | Potential Windows client | To verify | To verify | VMware inventory screenshot, 2026-11-20 |
| SPIDERMAN | Potential Windows client | To verify | To verify | VMware inventory screenshot, 2026-11-20 |

Other machines (including a Kali testing VM, if present) are not shown in this VMware screenshot; their presence and connectivity should not be assumed.

## Authorised scope

The user's own isolated virtual machines and intentionally configured lab services only. Third-party networks and systems are out of scope.

## Safety constraints

Use snapshots before intrusive tests. Do not upload passwords, hash values, tickets, real user details, privileged access tokens, sensitive screenshots or unnecessary external IP addresses. Redact nonessential environment identifiers before making the portfolio public.
