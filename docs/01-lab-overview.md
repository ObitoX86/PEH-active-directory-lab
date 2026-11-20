# Lab Overview and Scope

## Objective

Practise Windows Active Directory configuration, security assessment and mitigation in a controlled virtual environment as part of TCM Security's PEH course, and document evidence for a professional portfolio.

## Confirmed context

- A VMware Workstation inventory screenshot supplied on **2026-11-20** shows three VM entries: **Windows Server 2022**, **THEPUNISHER**, and **SPIDERMAN**. These are VMware display names, not independent proof of the installed operating systems or domain roles.
- During earlier lab troubleshooting, a Windows machine referred to as **Punisher** successfully accessed the lab Windows server through a direct UNC network path by private IP. The relationship between the earlier name and **THEPUNISHER** in the inventory remains to be confirmed.
- VMware inventory screenshot, server and both client `ipconfig /all` screenshots, `Get-ADDomainController` screenshot and both clients' `Get-CimInstance` domain-membership screenshots are available in the chat as evidence; all 14 corresponding redacted PNG evidence files are now stored in [the evidence index](../evidence/INDEX.md). VMware Settings evidence provided on 2026-11-20 confirms HYDRA-DC's VM Network Adapter is set to **NAT**, with **Connected** and **Connect at power on** checked. The user separately confirmed that THEPUNISHER and SPIDERMAN also use NAT; separate VMware Settings screenshots for these clients have not been provided.
- SPIDERMAN reports hostname `SPIDERMAN`, primary DNS suffix `MARVEL.local`, IPv4 `192.168.158.129/24`, DHCP server `192.168.158.254`, gateway `192.168.158.2`, and DNS server `192.168.158.128` (2026-11-20). A subsequent `Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain` query returned `SPIDERMAN`, `MARVEL.local`, `True` on 2026-11-20, verifying configured domain membership (not live authentication).
- THEPUNISHER reports IPv4 `192.168.158.130/24`, configured DNS server `192.168.158.128` (HYDRA-DC), and primary DNS suffix `MARVEL.local`. A separate `Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain` query returned `THEPUNISHER`, `MARVEL.local`, `True` on 2026-11-20, confirming the local computer reports domain membership (not independent proof of current authentication or domain controller reachability).
- The server's Windows configuration reports **host name `HYDRA-DC`** and **primary DNS suffix `MARVEL.local`**. The Active Directory PowerShell command `Get-ADDomainController -Identity HYDRA-DC` successfully returned `HYDRA-DC.MARVEL.local`, domain `MARVEL.local`, IPv4 `192.168.158.128`, and `IsGlobalCatalog=True` (2026-11-20). This verifies the directory identifies the machine as a domain controller and as a Global Catalog; individual services and client connectivity remain to be tested.

These observations do **not** establish administrator access or success of any penetration testing technique.

## VM inventory — based on VMware Workstation screenshot

| VMware display name | Expected purpose — not yet verified | Installed OS / domain role | Network settings / IP | Evidence |
| --- | --- | --- | --- | --- |
| Windows Server 2022 | Domain controller (verified by AD PowerShell query) | Guest hostname `HYDRA-DC`; directory lists it as a Global Catalog | IPv4 `192.168.158.128/24`; DHCP disabled; no default gateway; DNS `::1`, `127.0.0.1`; VMware adapter **NAT** | VMware inventory, `ipconfig /all` and `Get-ADDomainController` screenshots, 2026-11-20 |
| THEPUNISHER | Domain-joined Windows client (membership verified) | Guest hostname `THEPUNISHER`; `PartOfDomain=True` and domain `MARVEL.local` | IPv4 `192.168.158.130/24` (DHCP enabled); DNS `192.168.158.128`; gateway `192.168.158.2`; DHCP server `192.168.158.254`; VMware adapter NAT (user-confirmed) | `ipconfig /all` and `Get-CimInstance` screenshots, 2026-11-20 |
| SPIDERMAN | Domain-joined Windows client (membership verified) | Guest hostname `SPIDERMAN`; `PartOfDomain=True`, domain `MARVEL.local` | IPv4 `192.168.158.129/24` (DHCP enabled); DNS `192.168.158.128`; gateway `192.168.158.2`; DHCP server `192.168.158.254`; VMware adapter NAT (user-confirmed) | VMware inventory, `ipconfig /all`, and `Get-CimInstance` screenshots, 2026-11-20 |

Other machines (including a Kali testing VM, if present) are not shown in this VMware screenshot; their presence and connectivity should not be assumed.

**Screenshot references:** See [the 14-file evidence register](../evidence/INDEX.md), including server and client network settings, domain membership, NAT configuration, and connectivity diagnostics.

## Authorised scope

The user's own isolated virtual machines and intentionally configured lab services only. Third-party networks and systems are out of scope.

## Safety constraints

NAT is not a fully isolated or necessarily offline environment: verify containment before exercises that could generate disruptive traffic. Use snapshots before intrusive tests. Do not upload passwords, hash values, tickets, real user details, privileged access tokens, sensitive screenshots or unnecessary external IP addresses. Redact nonessential environment identifiers before making the portfolio public.
