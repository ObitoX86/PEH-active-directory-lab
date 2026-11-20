# Evidence batch 2 — planned upload (20 November 2026)

**Status: waiting for image upload.** The first 14 screenshot PNGs are already confirmed in this repository. These **26 new screenshots (15–40)** are prepared, named, and packaged from the user-supplied images, but are **not yet present in GitHub**. Do not mark them uploaded or embed broken links before verification.

| Filename to upload into `evidence/` | Screenshot evidence |
| --- | --- |
| `2026-11-20_15-ad-organizational-units.png` | OU enumeration |
| `2026-11-20_16-ad-group-inventory.png` | 20 AD groups |
| `2026-11-20_17-domain-admins-members.png` | Domain Admins membership |
| `2026-11-20_18-sqlservice-spn.png` | SQLService SPN |
| `2026-11-20_19-kali-initial-ip.png` | Kali initial IPv4 |
| `2026-11-20_20-kali-nat-adapter.png` | Kali NAT adapter |
| `2026-11-20_21-vmware-virtual-network-editor.png` | VMnet1 and VMnet8 |
| `2026-11-20_22-hydra-dc-isolated-ipconfig.png` | HYDRA-DC isolated IPv4 |
| `2026-11-20_23-kali-nmcli-active-profile.png` | Kali NetworkManager active profile |
| `2026-11-20_24-kali-nmcli-static-profile.png` | Kali saved static IPv4 |
| `2026-11-20_25-kali-isolated-vmware-adapter.png` | Kali LAN segment adapter |
| `2026-11-20_26-kali-isolated-ip-and-route.png` | Kali post-migration IP and routes |
| `2026-11-20_27-kali-to-hydra-dc-ping.png` | Kali to DC ping |
| `2026-11-20_28-thepunisher-adapter-name.png` | THEPUNISHER Ethernet0 |
| `2026-11-20_29-thepunisher-static-ip-before-move.png` | THEPUNISHER pre-migration static IP |
| `2026-11-20_30-thepunisher-isolated-ipconfig.png` | THEPUNISHER post-migration IP |
| `2026-11-20_31-thepunisher-isolated-tcp389.png` | THEPUNISHER LDAP TCP 389 |
| `2026-11-20_32-spiderman-adapter-name.png` | SPIDERMAN Ethernet0 |
| `2026-11-20_33-spiderman-static-ip-before-move.png` | SPIDERMAN pre-migration static IP |
| `2026-11-20_34-spiderman-isolated-ipconfig.png` | SPIDERMAN post-migration IP |
| `2026-11-20_35-spiderman-isolated-tcp389.png` | SPIDERMAN LDAP TCP 389 |
| `2026-11-20_36-spiderman-isolated-dns-a.png` | SPIDERMAN DNS A record |
| `2026-11-20_37-spiderman-isolated-dns-srv.png` | SPIDERMAN DNS SRV record |
| `2026-11-20_38-spiderman-isolated-dc-locator.png` | SPIDERMAN DC Locator |
| `2026-11-20_39-kali-isolated-ipv4-ipv6-routes.png` | Kali IPv4 and IPv6 routes |
| `2026-11-20_40-kali-no-external-ipv4-route.png` | Kali unreachable external IPv4 route |

## Upload instructions

1. Extract the prepared batch ZIP from this conversation.
2. In its `upload-to-evidence/` folder, select the **26 PNG files** (not the ZIP or Markdown files).
3. Open [GitHub evidence directory](https://github.com/ObitoX86/PEH-active-directory-lab/tree/main/evidence) and choose **Add file → Upload files**.
4. Drag all 26 PNG files, then commit with message `Add 26 PEH lab screenshots (15-40)`.
5. Once GitHub shows those files, update `evidence/INDEX.md` with inline `![description](filename.png)` embeds, and place relevant relative-image links beside corresponding findings in `docs/02-network-architecture.md`, `docs/03-assessment-journal.md` and `docs/06-active-directory-structure.md`.

## Quality and privacy

These screenshots illustrate authorised local lab topology, Active Directory inventory, and network connectivity. They do not demonstrate exploitation or recovered credentials. Review screenshots for anything you do not want public before changing repository visibility; do not commit real passwords, authentication tokens, or credential hashes. Separate explicit user confirmation of snapshot creation and four-VM adapter audit from screenshot-verified results.
