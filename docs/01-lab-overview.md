# Lab Overview and Scope

## Objective

Practise Active Directory administration, authorised security assessment and defensive mitigation in a VMware training environment, with a defensible portfolio record based on the TCM Security Practical Ethical Hacking course.

> **Configuration as of 20 November 2026:** The four guests are **user-reported** attached to the dedicated VMware LAN segment `PEH-AD-Isolated`. Their current guest IP configurations and selected internal connectivity tests are backed by screenshots. The separate VMware single-adapter audit for all four guests is user-confirmed; it is not independently screenshot-verified for every guest.

## Current lab inventory

| Guest | Role and domain evidence | Current observed IPv4 | Current IPv4 / DNS configuration | Evidence |
| --- | --- | --- | --- | --- |
| **HYDRA-DC** (VMware display name `Windows Server 2022`) | `Get-ADDomainController` reports DC and Global Catalog in `MARVEL.local` | `192.168.158.128/24` | DHCP disabled; no gateway; DNS `::1`, `127.0.0.1` | [DC identity](../evidence/2026-11-20_03-hydra-dc-ad-identity.png), [post-migration IP](../evidence/2026-11-20_22-hydra-dc-isolated-ipconfig.png) |
| **SPIDERMAN** | `Win32_ComputerSystem` reports `PartOfDomain=True`, `MARVEL.local`; this is not a live trust test | `192.168.158.129/24` | DHCP disabled; no gateway; DNS `192.168.158.128` | [post-migration IP](../evidence/2026-11-20_34-spiderman-isolated-ipconfig.png), [membership](../evidence/2026-11-20_07-spiderman-domain-membership.png) |
| **THEPUNISHER** | `Win32_ComputerSystem` reports `PartOfDomain=True`, `MARVEL.local` | `192.168.158.130/24` | DHCP disabled; no gateway; DNS `192.168.158.128` | [post-migration IP](../evidence/2026-11-20_30-thepunisher-isolated-ipconfig.png), [membership](../evidence/2026-11-20_05-thepunisher-domain-membership.png) |
| **Kali Linux** | Authorised test machine; `eth0` live and connected within `192.168.158.0/24` | `192.168.158.131/24` | Static IP; DNS `192.168.158.128` in saved profile; no default route displayed | [VMware LAN selection](../evidence/2026-11-20_25-kali-isolated-vmware-adapter.png), [routes](../evidence/2026-11-20_39-kali-isolated-ipv4-ipv6-routes.png) |

The VMware inventory screenshot originally showed only three display names: `Windows Server 2022`, `THEPUNISHER` and `SPIDERMAN`. Kali was documented separately. Display names are not independent proof of installed OS versions. The guest hostname `HYDRA-DC` and AD role are verified through separate Windows/AD outputs.

## Current verified network observations

- Kali → HYDRA-DC: `ping -c 4 192.168.158.128` returned **4/4 replies** ([image](../evidence/2026-11-20_27-kali-to-hydra-dc-ping.png)).
- THEPUNISHER → HYDRA-DC: TCP port **389** connection succeeded after migration ([image](../evidence/2026-11-20_31-thepunisher-isolated-tcp389.png)).
- SPIDERMAN → HYDRA-DC: TCP port **389** connection succeeded after migration ([image](../evidence/2026-11-20_35-spiderman-isolated-tcp389.png)).
- SPIDERMAN resolved HYDRA-DC's DNS **A** and AD LDAP **SRV** records and completed forced Windows DC Locator ([A](../evidence/2026-11-20_36-spiderman-isolated-dns-a.png), [SRV](../evidence/2026-11-20_37-spiderman-isolated-dns-srv.png), [Locator](../evidence/2026-11-20_38-spiderman-isolated-dc-locator.png)).
- Kali's inspected IPv4 and IPv6 routes showed **no default routes**; a lookup of `1.1.1.1` returned **Network is unreachable** ([routes](../evidence/2026-11-20_39-kali-isolated-ipv4-ipv6-routes.png), [lookup](../evidence/2026-11-20_40-kali-no-external-ipv4-route.png)).

These support the intended local-only lab topology but do **not** demonstrate full hypervisor/host isolation, authenticated LDAP operations, or Kerberos login. SPIDERMAN's repeated `Test-ComputerSecureChannel=False` remains unresolved despite a prior `nltest /sc_query` last-used success report; see [issue #1](https://github.com/ObitoX86/PEH-active-directory-lab/issues/1).

## Historical NAT configuration — superseded

Initial 20 November screenshots captured the machines on VMware **NAT/VMnet8**, using the `192.168.158.0/24` subnet. At that earlier stage, THEPUNISHER and SPIDERMAN had **DHCP enabled**, gateway `192.168.158.2`, DHCP server `192.168.158.254`, and DNS `192.168.158.128`. HYDRA-DC already had static `.128` addressing. Kali initially had a NAT adapter. VMnet1 was separately observed as host-only on `192.168.125.0`.

Those records are **historical observations**, not today's settings. The later migration retained the same guest IP addresses on a **different, separately named LAN segment** while replacing client DHCP with static addressing and removing default gateways. See [network architecture](02-network-architecture.md) and the dated [assessment journal](03-assessment-journal.md).

An earlier file-sharing troubleshooting session found a Windows client called *Punisher* could access a lab server using a direct UNC path by private IP. Its exact relationship to `THEPUNISHER` and the earlier server IP remains unconfirmed; do not treat this as a current share or authentication test.

## Scope and limitations

**Authorised targets:** Only the user's controlled `HYDRA-DC`, `SPIDERMAN`, `THEPUNISHER` and Kali virtual lab systems. Third-party and production systems are out of scope.

**Completed:** Configuration evidence, selected transport/DNS/DC discovery checks, initial AD OU and group enumeration, and identification of an intentionally privileged SPN-bearing service account.

**Not established:** Working end-to-end domain authentication, healthy SPIDERMAN machine trust, successful attack techniques, credential extraction, attack impact, or remediations independently retested.

**Evidence and safety:** [All 40 PNG files](../evidence/INDEX.md) have been uploaded. Preserve screenshots and dates as recorded, distinguish user confirmation from direct screen evidence, and do not publish credentials, captured hashes, tickets, private identifiers or inappropriate live-system details. The repository is currently public, so review both screenshots and commit history before sharing it further.
