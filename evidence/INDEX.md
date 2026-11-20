# Screenshot Evidence Register — 20 November 2026

**Status: PNG files pending repository upload.** The filenames below refer to the 14 redacted PNG images already prepared from this project's screenshots. The technical outcomes are separately documented in [the assessment journal](../docs/03-assessment-journal.md) and [the network architecture document](../docs/02-network-architecture.md).

| File | Evidence of |
| --- | --- |
| `2026-11-20_01-vmware-inventory.png` | Three VMware lab machines |
| `2026-11-20_02-hydra-dc-ipconfig.png` | HYDRA-DC IPv4, subnet and DNS configuration |
| `2026-11-20_03-hydra-dc-ad-identity.png` | Domain controller / Global Catalog PowerShell lookup |
| `2026-11-20_04-thepunisher-ipconfig.png` | THEPUNISHER IP and DNS configuration |
| `2026-11-20_05-thepunisher-domain-membership.png` | THEPUNISHER configured domain membership |
| `2026-11-20_06-spiderman-ipconfig.png` | SPIDERMAN IP and DNS configuration |
| `2026-11-20_07-spiderman-domain-membership.png` | SPIDERMAN configured domain membership |
| `2026-11-20_08-hydra-dc-vmware-nat.png` | HYDRA-DC VMware NAT adapter selection |
| `2026-11-20_09-thepunisher-dns-lookup.png` | HYDRA-DC A-record resolution from THEPUNISHER |
| `2026-11-20_10-thepunisher-ldap-tcp389.png` | THEPUNISHER TCP port 389 connectivity |
| `2026-11-20_11-spiderman-ldap-tcp389.png` | SPIDERMAN TCP port 389 connectivity |
| `2026-11-20_12-spiderman-dc-discovery.png` | SPIDERMAN `nltest /dsgetdc` result |
| `2026-11-20_13-spiderman-secure-channel-failure.png` | SPIDERMAN initial `Test-ComputerSecureChannel=False` |
| `2026-11-20_14-spiderman-netlogon-sc-query.png` | SPIDERMAN `nltest /sc_query` last-used Netlogon success status |

These screenshots support the particular displayed facts, **not** unsupported conclusions about live authentication, trust health, or exploitation. The repeat elevated `False` secure-channel result was confirmed in chat but has no separate screenshot.

## Repository upload

The 14 PNG files should be placed directly in the `evidence/` directory with these exact names. Before making this repository public, inspect the redactions again. Update this register to link each uploaded PNG only after the files actually appear in GitHub. 
