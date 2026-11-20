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
- [x] Upload 14 redacted lab baseline screenshots and verify all are present in `evidence/` (2026-11-20).
- [ ] Assess NAT network containment before intrusive testing; NAT is not fully isolated.
- [x] Verify THEPUNISHER resolves `HYDRA-DC.MARVEL.local` to `192.168.158.128` (DNS A answer, TTL 3600), 2026-11-20.
- [x] Test THEPUNISHER to HYDRA-DC TCP port 389 connectivity (`TcpTestSucceeded=True`, 2026-11-20).
- [x] Test SPIDERMAN to HYDRA-DC TCP port 389 connectivity (`TcpTestSucceeded=True`, 2026-11-20).
- [ ] Confirm authenticated Active Directory service operation separately; TCP connectivity is not authentication.
- [ ] Verify guest operating systems, other VM roles and individual service health.
- [x] Document domain controller and both visible clients (hostnames, addresses, AD membership) from supplied evidence.
- [x] Identify Kali Linux testing environment via `ip -br addr` (`eth0` UP, `192.168.158.131/24`; 2026-11-20).
- [x] Verify Kali VMware Network Adapter is NAT and connected at power-on (Settings screenshot, 2026-11-20).
- [x] Identify VMware NAT VMnet8 (`192.168.158.0`) and host-only VMnet1 (`192.168.125.0`) from Virtual Network Editor (2026-11-20).
- [ ] Test scoped Kali-to-Windows connectivity and record containment; NAT alone does not establish isolation.
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
- [ ] Upload the OU enumeration screenshot as redacted evidence when preparing the next screenshot batch.

## Transition to PEH Active Directory initial attack vectors
- [x] Identify Kali Linux as intended authorised tester at `192.168.158.131/24`.
- [x] Confirm Kali's hypervisor Network Adapter is NAT (screenshot).
- [x] Identify VMnet8 as the NAT subnet (VMware Virtual Network Editor screenshot).
- [x] User confirmed creating VMware snapshot `PEH-Kali-Baseline-Before-Attacks` for Kali Linux (2026-11-20; confirmation in conversation, snapshot not independently inspected).
- [x] User confirmed creating powered-off VMware snapshot `PEH-HYDRA-DC-Baseline-Before-Attacks` for HYDRA-DC (2026-11-20; not independently inspected).
- [x] User confirmed powered-off VMware snapshot `PEH-THEPUNISHER-Baseline-Before-Attacks` for THEPUNISHER (2026-11-20; not independently inspected).
- [x] User confirmed powered-off VMware snapshot `PEH-SPIDERMAN-Baseline-Before-Attacks` for SPIDERMAN (2026-11-20; not independently inspected; existing secure-channel problem retained).
- [x] User verified all four expected baseline snapshots are visible in VMware Snapshot Manager (2026-11-20; user-confirmed, no manager screenshots).
- [x] User confirmed creation of VMware LAN segment `PEH-AD-Isolated` on 2026-11-20; no screenshot verification or VM migration yet.
- [x] Draft guest-address preservation plan for `PEH-AD-Isolated`: HYDRA-DC `.128`, SPIDERMAN `.129`, THEPUNISHER `.130`, Kali `.131`, all `/24`; preserve HYDRA-DC DNS for clients; no gateway needed in contained LAN.
- [x] User followed powered-off HYDRA-DC migration to `PEH-AD-Isolated` and supplied `ipconfig /all` confirming static `192.168.158.128/24`, DHCP disabled, no gateway, DNS loopback, `MARVEL.local` (2026-11-20). VMware segment selection is user-reported, not separately screenshot-verified.
- [x] Identify Kali's active NetworkManager connection as `Wired connection 1` on `eth0` (2026-11-20 screenshot from `nmcli -f NAME,DEVICE connection show --active`).
- [x] User confirmed running `nmcli connection modify` for Kali `Wired connection 1` with manual IPv4 `192.168.158.131/24`, DNS `192.168.158.128`, no gateway/default route (2026-11-20; settings not yet independently read back).
- [x] Screenshot-verify Kali's saved `Wired connection 1` profile: `manual`, `192.168.158.131/24`, gateway empty, DNS `192.168.158.128`, `never-default=yes` (2026-11-20). Saved profile verified, active network state not yet verified.
- [x] User switched Kali VMware adapter to LAN segment `PEH-AD-Isolated` and supplied post-boot `ip -br addr && ip route` confirming `eth0` UP at `192.168.158.131/24`, only `192.168.158.0/24` connected IPv4 route and no IPv4 default route (2026-11-20). Adapter selection user-reported/screenshot prior to boot; guest routing screenshot-verified.
- [x] Verify Kali-to-HYDRA-DC ICMP reachability on the isolated LAN: `ping -c 4 192.168.158.128` returned 4/4 replies, 0% loss, avg RTT 0.337 ms (2026-11-20 screenshot). This establishes ICMP reachability only.
- [ ] Verify DNS, other NICs and IPv6/IPv4 routes before claiming full containment.
- [x] Verify THEPUNISHER's current Windows network adapter is `Ethernet0`, status `Up`, hardware `Intel(R) 82574L Gigabit Network Connection` via `Get-NetAdapter` screenshot (2026-11-20).
- [x] User confirmed executing `netsh interface ipv4 set address name="Ethernet0" static 192.168.158.130 255.255.255.0 none` on THEPUNISHER (2026-11-20; output not yet independently verified).
- [x] Screenshot-verify THEPUNISHER post-change `ipconfig /all`: hostname `THEPUNISHER`, DNS suffix `MARVEL.local`, IPv4 `192.168.158.130/24`, DHCP `No`, gateway blank, DNS `192.168.158.128` (2026-11-20). No further IPv4/DNS change needed before VMware segment migration.
- [ ] Move THEPUNISHER to `PEH-AD-Isolated`, verify address and DC connectivity.
- [ ] Move SPIDERMAN and configure static `192.168.158.129/24` with DNS `.128`; verify the full lab works on the isolated LAN.
- [ ] Verify guest isolation, correct IP/DNS and scoped connectivity on LAN segment before any traffic-interception test.
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
