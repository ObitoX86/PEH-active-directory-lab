# Portfolio Roadmap

Each phase remains **not verified** until relevant lab evidence is recorded.

> **Current checkpoint (21 November 2026):** All four VMs were user-reported migrated to VMware LAN Segment `PEH-AD-Isolated` on 20 November; current guest addressing and selected internal paths were screenshot-verified. **40/40** PNGs are uploaded. Initial PEH attack execution, full isolation assurance, domain authentication and SPIDERMAN secure-channel repair are **not** verified. Earlier NAT/DHCP checkpoints below are *historical milestones*, not today's configuration. See [current topology](docs/02-network-architecture.md) and [evidence register](evidence/INDEX.md).

## Phase 1 — Build and inventory (includes historical NAT baseline)
- [x] Capture and identify VMware display names of three VMs: Windows Server 2022, THEPUNISHER, SPIDERMAN (screenshot supplied 2026-11-20).
- [x] Record `HYDRA-DC` guest hostname, DNS suffix and IPv4/DNS configuration from server `ipconfig /all` (2026-11-20).
- [x] Verify HYDRA-DC domain controller identity and Global Catalog via `Get-ADDomainController` (2026-11-20).
- [x] Record THEPUNISHER's IP, DNS, DHCP, gateway and hostname from `ipconfig /all` (2026-11-20).
- [x] Verify THEPUNISHER's domain membership in `MARVEL.local` using `Win32_ComputerSystem` (`PartOfDomain=True`), 2026-11-20.
- [x] Record SPIDERMAN's IP, DNS, DHCP, gateway and hostname from `ipconfig /all` (2026-11-20).
- [x] Verify SPIDERMAN's configured domain membership in `MARVEL.local` using `Win32_ComputerSystem` (`PartOfDomain=True`), 2026-11-20.
- [x] Verify HYDRA-DC VMware Network Adapter is NAT, connected and enabled at power-on (VMware Settings screenshot, 2026-11-20).
- [x] Record user confirmation that THEPUNISHER and SPIDERMAN VMware adapters use NAT (2026-11-20; separate client Settings screenshots not supplied).
- [x] Upload initial 14 redacted baseline screenshots (2026-11-20); subsequently uploaded screenshots 15–40, for **40 total** in `evidence/`.
- [x] Retire NAT as the planned attack-test network; the lab was subsequently moved to `PEH-AD-Isolated`. NAT containment was **not** independently tested and is not assumed.
- [x] Verify THEPUNISHER resolves `HYDRA-DC.MARVEL.local` to `192.168.158.128` (DNS A answer, TTL 3600), 2026-11-20.
- [x] Test THEPUNISHER to HYDRA-DC TCP port 389 connectivity (`TcpTestSucceeded=True`, 2026-11-20).
- [x] Test SPIDERMAN to HYDRA-DC TCP port 389 connectivity (`TcpTestSucceeded=True`, 2026-11-20).
- [ ] Confirm authenticated Active Directory service operation separately; TCP connectivity is not authentication.
- [ ] Verify guest operating systems, other VM roles and individual service health.
- [x] Document domain controller and both visible clients (hostnames, addresses, AD membership) from supplied evidence.
- [x] Identify Kali Linux testing environment via `ip -br addr` (`eth0` UP, `192.168.158.131/24`; 2026-11-20).
- [x] Verify Kali VMware Network Adapter is NAT and connected at power-on (Settings screenshot, 2026-11-20).
- [x] Identify VMware NAT VMnet8 (`192.168.158.0`) and host-only VMnet1 (`192.168.125.0`) from Virtual Network Editor (2026-11-20).
- [x] Test Kali → HYDRA-DC ICMP on the isolated LAN (4/4 replies) and record later route/adapter checks. Kali connectivity to both Windows clients has **not** separately been tested; full host/hypervisor containment remains unproven.
- [x] Create a Mermaid network topology with verified IP addresses, domain roles and both tested TCP 389 paths (2026-11-20).
- [x] Confirm domain controller discovery from SPIDERMAN using `nltest /dsgetdc:MARVEL.local` (returned HYDRA-DC at `192.168.158.128`, 2026-11-20).
- [x] Run SPIDERMAN computer secure-channel diagnostic: `Test-ComputerSecureChannel -Verbose` returned **False** with a broken-channel message (2026-11-20).
- [x] Run `nltest /sc_query:MARVEL.local` on SPIDERMAN: last-used Netlogon channel reports `0x0 NERR_Success` with HYDRA-DC (2026-11-20).
- [x] User repeated `Test-ComputerSecureChannel -Verbose` in elevated PowerShell; still returned **False** (user-reported, 2026-11-20).
- [ ] Diagnose and resolve SPIDERMAN computer secure-channel issue when required; deferred, tracked in [issue #1](https://github.com/ObitoX86/PEH-active-directory-lab/issues/1). Do not mark healthy.
- [ ] Verify computer secure-channel health and live authentication after diagnostics; domain controller discovery is not proof of either.
- [x] Establish at least one successful client-to-server file-sharing connection by direct UNC path (observed during prior lab troubleshooting).

## Active Directory object structure
- [x] Inventory OUs with `Get-ADOrganizationalUnit -Filter *`: `Domain Controllers` and `Groups` (2026-11-20).
- [x] Inspect 20 direct child objects in `OU=Groups`; all returned with `ObjectClass=group` (2026-11-20).
- [x] Verify `Domain Admins` direct membership (3 user objects: `Administrator`, `Tony Stark`, `SQL Service`; 2026-11-20).
- [x] Verify `SQL Service` has `SamAccountName=SQLService` and registered SPN `HYDRA-DC/SQLService.MARVEL.local:60111` (2026-11-20).
- [x] Record user's confirmation that privileged SQLService + SPN configuration was **intentional PEH lab setup** (2026-11-20).
- [ ] Test actual Kerberoasting behaviour only in an authorised, contained lab; no ticket request or credential recovery has yet been documented.
- [ ] Document relevant users, security groups, computer objects and Group Policy links based on verified results.
- [x] Upload OU enumeration as evidence screenshot `2026-11-20_15-ad-organizational-units.png` (included in 40-file register).

## Transition to PEH Active Directory initial attack vectors
- [x] Identify Kali Linux as intended authorised tester at `192.168.158.131/24`.
- [x] Confirm Kali's hypervisor Network Adapter is NAT (screenshot).
- [x] Identify VMnet8 as the NAT subnet (VMware Virtual Network Editor screenshot).
- [x] User confirmed creating VMware snapshot `PEH-Kali-Baseline-Before-Attacks` for Kali Linux (2026-11-20; confirmation in conversation, snapshot not independently inspected).
- [x] User confirmed creating powered-off VMware snapshot `PEH-HYDRA-DC-Baseline-Before-Attacks` for HYDRA-DC (2026-11-20; not independently inspected).
- [x] User confirmed powered-off VMware snapshot `PEH-THEPUNISHER-Baseline-Before-Attacks` for THEPUNISHER (2026-11-20; not independently inspected).
- [x] User confirmed powered-off VMware snapshot `PEH-SPIDERMAN-Baseline-Before-Attacks` for SPIDERMAN (2026-11-20; not independently inspected; existing secure-channel problem retained).
- [x] User verified all four expected baseline snapshots are visible in VMware Snapshot Manager (2026-11-20; user-confirmed, no manager screenshots).
- [x] User confirmed creating LAN segment `PEH-AD-Isolated` on 2026-11-20 (creation itself not screenshot-verified); all four VMs were **subsequently** reported migrated, with current guest network evidence recorded below.
- [x] Draft guest-address preservation plan for `PEH-AD-Isolated`: HYDRA-DC `.128`, SPIDERMAN `.129`, THEPUNISHER `.130`, Kali `.131`, all `/24`; preserve HYDRA-DC DNS for clients; no gateway needed in contained LAN.
- [x] User followed powered-off HYDRA-DC migration to `PEH-AD-Isolated` and supplied `ipconfig /all` confirming static `192.168.158.128/24`, DHCP disabled, no gateway, DNS loopback, `MARVEL.local` (2026-11-20). VMware segment selection is user-reported, not separately screenshot-verified.
- [x] Identify Kali's active NetworkManager connection as `Wired connection 1` on `eth0` (2026-11-20 screenshot from `nmcli -f NAME,DEVICE connection show --active`).
- [x] User confirmed running `nmcli connection modify` for Kali `Wired connection 1` with manual IPv4 `192.168.158.131/24`, DNS `192.168.158.128`, no gateway/default route (2026-11-20; saved settings later verified by screenshot).
- [x] Screenshot-verify Kali's saved `Wired connection 1` profile: `manual`, `192.168.158.131/24`, gateway empty, DNS `192.168.158.128`, `never-default=yes` (2026-11-20). Saved profile verified; live post-migration `eth0` state was verified in a later step.
- [x] User switched Kali VMware adapter to LAN segment `PEH-AD-Isolated` and supplied post-boot `ip -br addr && ip route` confirming `eth0` UP at `192.168.158.131/24`, only `192.168.158.0/24` connected IPv4 route and no IPv4 default route (2026-11-20). Adapter selection user-reported/screenshot prior to boot; guest routing screenshot-verified.
- [x] Verify Kali-to-HYDRA-DC ICMP reachability on the isolated LAN: `ping -c 4 192.168.158.128` returned 4/4 replies, 0% loss, avg RTT 0.337 ms (2026-11-20 screenshot). This establishes ICMP reachability only.
- [ ] Independently corroborate full host/hypervisor isolation before claiming it. Kali IPv4/IPv6 routes and direct AD DNS queries were screenshot-verified; a single-NIC `PEH-AD-Isolated` configuration on all four guests was user-confirmed but not fully screenshot-verified.
- [x] Verify THEPUNISHER's current Windows network adapter is `Ethernet0`, status `Up`, hardware `Intel(R) 82574L Gigabit Network Connection` via `Get-NetAdapter` screenshot (2026-11-20).
- [x] User confirmed executing `netsh interface ipv4 set address name="Ethernet0" static 192.168.158.130 255.255.255.0 none` on THEPUNISHER (2026-11-20; static settings subsequently read back in screenshots).
- [x] Screenshot-verify THEPUNISHER post-change `ipconfig /all`: hostname `THEPUNISHER`, DNS suffix `MARVEL.local`, IPv4 `192.168.158.130/24`, DHCP `No`, gateway blank, DNS `192.168.158.128` (2026-11-20). No further IPv4/DNS change needed before VMware segment migration.
- [x] User moved THEPUNISHER VMware adapter to `PEH-AD-Isolated` and provided post-boot `ipconfig /all` screenshot confirming IPv4 `192.168.158.130/24`, DHCP No, blank gateway, DNS `192.168.158.128`, domain suffix `MARVEL.local` (2026-11-20). VMware segment selection is user-reported rather than separately screenshot-verified.
- [x] Verify THEPUNISHER-to-HYDRA-DC TCP 389 connectivity after isolated-LAN migration: `Test-NetConnection 192.168.158.128 -Port 389` returned `TcpTestSucceeded: True`, source `192.168.158.130` via `Ethernet0` (2026-11-20 screenshot). LDAP authentication not yet verified.
- [x] Verify SPIDERMAN's interface name `Ethernet0`, status `Up`, Intel(R) 82574L Gigabit Network Connection from `Get-NetAdapter` screenshot (2026-11-20).
- [x] User confirmed running `netsh interface ipv4 set address name="Ethernet0" static 192.168.158.129 255.255.255.0 none` on SPIDERMAN (2026-11-20); later `ipconfig /all` screenshots verified these static settings.
- [x] Screenshot-verify SPIDERMAN's `ipconfig /all` before isolated LAN migration: `192.168.158.129/24`, DHCP No, default gateway blank, DNS `192.168.158.128`, domain suffix `MARVEL.local` (2026-11-20).
- [x] User reports moving SPIDERMAN to `PEH-AD-Isolated`; its post-boot `ipconfig /all` screenshot confirms `192.168.158.129/24`, DHCP disabled, blank gateway, DNS `192.168.158.128` and `MARVEL.local` (2026-11-20). VMware adapter selection not separately screenshot-verified.
- [x] Screenshot-verify SPIDERMAN-to-HYDRA-DC TCP 389 connection after isolated-LAN migration: `SourceAddress=192.168.158.129`, `RemoteAddress=192.168.158.128`, `InterfaceAlias=Ethernet0`, `TcpTestSucceeded=True` (2026-11-20). Transport only, not an authenticated LDAP bind.
- [x] Screenshot-verify SPIDERMAN's isolated-LAN DNS A query: `Resolve-DnsName HYDRA-DC.MARVEL.local -Server 192.168.158.128 -Type A` returned `192.168.158.128`, TTL 3600, Answer (2026-11-20). Confirms direct A lookup only; secure-channel issue remains separate.
- [x] Screenshot-verify SPIDERMAN's AD LDAP discovery SRV query `_ldap._tcp.dc._msdcs.MARVEL.local` to DNS `192.168.158.128`: target `hydra-dc.marvel.local`, port 389, priority 0, weight 100, TTL 600, and additional A record `192.168.158.128` TTL 3600 (2026-11-20).
- [x] Screenshot-verify SPIDERMAN's forced DC locator: `nltest /dsgetdc:MARVEL.local /force` returned `\\HYDRA-DC.MARVEL.local`, `\\192.168.158.128`, forest/domain `MARVEL.local`, site `Default-First-Site-Name`, flags including PDC/GC/LDAP/KDC/DNS_DC/WRITABLE, and `The command completed successfully` (2026-11-20). DC discovery works; client computer secure-channel remains separately unresolved.
- [x] Screenshot-verify Kali routing after isolated LAN migration: `ip -4 route && ip -6 route` shows only connected IPv4 route `192.168.158.0/24 dev eth0` (source `192.168.158.131`, metric 100) and IPv6 link-local `fe80::/64 dev eth0` (metric 1024). No IPv4 or IPv6 default route appears (2026-11-20). This confirms Kali's routing configuration, not complete VMware isolation.
- [x] Screenshot-verify Kali's external IPv4 route lookup: `ip -4 route get 1.1.1.1` returned `RTNETLINK answers: Network is unreachable` (2026-11-20). This read-only kernel lookup does not send network packets and confirms no usable IPv4 route to that destination at the time.
- [x] User confirmed VMware Workstation adapter audit for all four guests: one network adapter per VM, each connected to LAN Segment `PEH-AD-Isolated`, with no extra NAT/Bridged/Host-only adapter (2026-11-20, user confirmation; no new screenshots).
- [ ] Verify all four VMs operate correctly on the isolated LAN; do not infer AD authentication from network settings alone.
- [x] Document planned private lab scope: HYDRA-DC `.128`, SPIDERMAN `.129`, THEPUNISHER `.130`, Kali `.131` on `192.168.158.0/24`; VMware adapter audit user-confirmed; lab-only connectivity and DC discovery tested. No attack traffic yet.
- [ ] Run a passive, non-poisoning Responder analysis exercise on Kali before considering an active lab demonstration.
- [ ] Verify containment and approved scope before active attack simulation.
- [ ] Study LLMNR/NBT-NS poisoning prerequisites and mitigations before running Responder in an isolated lab.
- [ ] Keep SPIDERMAN trust-status anomaly separate from attack findings; investigate only if a lab depends on healthy computer trust.

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
