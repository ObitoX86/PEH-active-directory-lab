# Lab Overview and Scope

## Objective

Practise Windows Active Directory configuration, security assessment and mitigation in an isolated environment as part of TCM Security's PEH course, and document evidence for a professional portfolio.

## Confirmed context

- A VMware Workstation inventory screenshot supplied on **2026-11-20** shows three VM entries: **Windows Server 2022**, **THEPUNISHER**, and **SPIDERMAN**. These are VMware display names, not independent proof of the installed operating systems or domain roles.
- During earlier lab troubleshooting, a Windows machine referred to as **Punisher** successfully accessed the lab Windows server through a direct UNC network path by private IP. The relationship between the earlier name and **THEPUNISHER** in the inventory remains to be confirmed.
- VMware inventory screenshot is available in the chat as evidence; a redacted copy has not yet been committed to the repository.

These observations do **not** establish administrator access or success of any penetration testing technique.

## VM inventory — based on VMware Workstation screenshot

| VMware display name | Expected purpose — not yet verified | Installed OS / domain role | Network settings / IP | Evidence |
| --- | --- | --- | --- | --- |
| Windows Server 2022 | Potential domain controller | To verify | To verify | VMware inventory screenshot, 2026-11-20 |
| THEPUNISHER | Potential Windows client | To verify | To verify | VMware inventory screenshot, 2026-11-20 |
| SPIDERMAN | Potential Windows client | To verify | To verify | VMware inventory screenshot, 2026-11-20 |

Other machines (including a Kali testing VM, if present) are not shown in this VMware screenshot; their presence and connectivity should not be assumed.

## Authorised scope

The user's own isolated virtual machines and intentionally configured lab services only. Third-party networks and systems are out of scope.

## Safety constraints

Use snapshots before intrusive tests. Do not upload passwords, hash values, tickets, real user details, privileged access tokens, sensitive screenshots or unnecessary external IP addresses. Redact nonessential environment identifiers before making the portfolio public.
