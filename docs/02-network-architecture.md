# Network Architecture

## Observed machines

The VMware Workstation inventory screenshot (2026-11-20) contains three display names:

```text
VMware Workstation — My Computer
  - Windows Server 2022
  - THEPUNISHER
  - SPIDERMAN
```

The displayed names alone do not prove host roles, domain membership or network links.

## Verified server network configuration — `ipconfig /all` screenshot, 2026-11-20

| Field | Observed value |
| --- | --- |
| VMware display name | Windows Server 2022 |
| Windows host name | `HYDRA-DC` |
| Primary DNS suffix | `MARVEL.local` |
| Ethernet adapter description | Intel(R) 82574L Gigabit Network Connection |
| IPv4 address | `192.168.158.128` (preferred) |
| Subnet mask | `255.255.255.0` (`/24`) |
| IPv4 network | `192.168.158.0/24` (calculated from mask) |
| DHCP enabled | No |
| Default gateway | None shown |
| DNS servers | `::1` and `127.0.0.1` (loopback) |
| IP routing enabled | No |

**Interpretation:** DHCP is disabled; an IPv4 address is configured. The DNS servers are loopback addresses. An additional successful `Get-ADDomainController` query verifies the server is identified as an AD domain controller and Global Catalog; it does not independently test DNS availability or every AD service. The server's VMware adapter mode was subsequently verified as **NAT** by screenshot; the user confirmed **NAT** for both client adapters without providing separate Settings screenshots. Both clients have addresses in the same IPv4 subnet.

**Prior observation caveat:** An earlier troubleshooting session used a direct UNC path to `192.168.10.10`, whereas the current screenshot reports `192.168.158.128` for `HYDRA-DC`. Do not assume these are the same host/interface or that the network was changed without confirming the configuration.

## Verified VMware adapter mode — HYDRA-DC (VMware Settings screenshot, 2026-11-20)

| Property | Verified setting |
| --- | --- |
| VM display name | Windows Server 2022 (guest: `HYDRA-DC`) |
| VMware Network Adapter | **NAT: Used to share the host's IP address** |
| Connected | Checked |
| Connect at power on | Checked |

**Interpretation:** NAT normally allows the guest to share the host's network connection and may permit outbound connectivity. This does **not** guarantee an isolated or offline assessment environment. The user has also confirmed that THEPUNISHER and SPIDERMAN are configured for NAT; this is user-reported rather than screenshot-verified. The inactive `VMnet0` field in the screenshot is not evidence that the selected NAT adapter uses that virtual network.

**Assessment safety:** Before running intrusive or noisy training exercises, confirm all participating VMs' adapter modes and intended connectivity. Avoid changing the working network before documenting the configuration and assessing the impact on DNS, DHCP and the lab domain.

## Verified AD domain controller identity — PowerShell screenshot, 2026-11-20

```powershell
Get-ADDomainController -Identity HYDRA-DC |
    Select-Object HostName, Domain, IPv4Address, IsGlobalCatalog
```

| Returned property | Value |
| --- | --- |
| HostName | `HYDRA-DC.MARVEL.local` |
| Domain | `MARVEL.local` |
| IPv4Address | `192.168.158.128` |
| IsGlobalCatalog | `True` |

**Interpretation:** The successful query establishes that Active Directory recognises `HYDRA-DC` as a domain controller with the Global Catalog flag enabled. It is not a substitute for client DNS, logon or service-health testing. Evidence screenshot remains in the chat pending redacted upload.

## Verified client network configuration — THEPUNISHER (`ipconfig /all` screenshot, 2026-11-20)

| Field | Observed value |
| --- | --- |
| VMware display name / host name | `THEPUNISHER` |
| Primary DNS suffix | `MARVEL.local` |
| Connection-specific DNS suffix | `localdomain` |
| IPv4 address | `192.168.158.130` (preferred) |
| Subnet mask | `255.255.255.0` (`/24`) |
| DHCP enabled | Yes |
| Default gateway | `192.168.158.2` |
| DHCP server | `192.168.158.254` |
| DNS server | `192.168.158.128` |
| Primary WINS server | `192.168.158.2` |
| NetBIOS over TCP/IP | Enabled |

**Comparison:** The client and HYDRA-DC (`192.168.158.128/24`) are configured in the same IPv4 subnet (`192.168.158.0/24`). The client's configured DNS address matches the domain controller's IPv4 address. This establishes intended DNS configuration, **not** a successful DNS lookup or end-to-end network connectivity. Domain membership was later verified separately with a local Windows CIM query. DHCP service is configured at a separate address (`192.168.158.254`). VMware NAT mode has been confirmed for the server (screenshot) and both clients (user report).

## Verified THEPUNISHER domain membership — PowerShell screenshot, 2026-11-20

```powershell
Get-CimInstance Win32_ComputerSystem |
    Select-Object Name, Domain, PartOfDomain
```

| Property | Value |
| --- | --- |
| Name | `THEPUNISHER` |
| Domain | `MARVEL.local` |
| PartOfDomain | `True` |

**Interpretation:** The client computer reports that it is joined to the `MARVEL.local` domain. This verifies configured domain membership; active communication with the domain controller, live authentication and current Group Policy operation require further testing.

## Verified SPIDERMAN network configuration — `ipconfig /all` screenshot, 2026-11-20

| Field | Observed value |
| --- | --- |
| VMware display name / host name | `SPIDERMAN` |
| Primary DNS suffix | `MARVEL.local` |
| Connection-specific DNS suffix | `localdomain` |
| IPv4 address | `192.168.158.129` (preferred) |
| Subnet mask | `255.255.255.0` (`/24`) |
| DHCP enabled | Yes |
| Default gateway | `192.168.158.2` |
| DHCP server | `192.168.158.254` |
| DNS server | `192.168.158.128` |
| Primary WINS server | `192.168.158.2` |
| NetBIOS over TCP/IP | Enabled |

**Comparison:** SPIDERMAN is configured within `192.168.158.0/24` alongside HYDRA-DC and THEPUNISHER. Its configured DNS server matches HYDRA-DC's IPv4 address. DHCP assigns the client configuration from `192.168.158.254`. This proves the observed settings, **not** working DNS or network reachability. Domain membership was verified separately using a CIM query.

## Verified SPIDERMAN domain membership — PowerShell screenshot, 2026-11-20

```powershell
Get-CimInstance Win32_ComputerSystem |
    Select-Object Name, Domain, PartOfDomain
```

| Property | Value |
| --- | --- |
| Name | `SPIDERMAN` |
| Domain | `MARVEL.local` |
| PartOfDomain | `True` |

**Interpretation:** The local Windows computer reports that SPIDERMAN is joined to the `MARVEL.local` domain. This demonstrates configured membership, not a fresh successful domain authentication. Screenshot was supplied in the project conversation, not committed as an image.

## Verified connection from earlier troubleshooting

A Windows client referred to as **Punisher** accessed a lab Windows server using a direct UNC path by private IP. This is evidence of that specific connection; it does not verify all three VMware entries, the domain setup, or network discovery settings. The exact relationship between the previously reported client and **THEPUNISHER** is to be confirmed.

## Configuration diagram (IP/DNS/domain from screenshots; NAT for clients user-confirmed)

```text
          [HYDRA-DC]                    AD domain controller + GC; VMware adapter: NAT
          192.168.158.128/24             Domain: MARVEL.local
                 ^
                 | Configured as DNS server by both clients
          +------+----------------------+
          |                             |
   [THEPUNISHER]                   [SPIDERMAN]
   192.168.158.130/24              192.168.158.129/24
   Domain joined: YES              Domain joined: YES
   DHCP enabled                    DHCP enabled
   Gateway: 192.168.158.2          Gateway: 192.168.158.2

   All three IPv4 settings fall within 192.168.158.0/24.
   Diagram reflects configuration, NOT tested connectivity.
   All three VMs: NAT (server screenshot; clients user report).
   Virtual NAT switch identity and actual connectivity: TO VERIFY.
```

No connections are drawn until their configuration is confirmed.

## Details to confirm

1. Installed operating systems and purposes for each of the three VMs.
2. Which additional domain controller services are functioning (AD controller identity is verified).
3. Additional domain configuration and DNS health (domain identified as `MARVEL.local`; publish only if safe).
4. Screenshot evidence for client NAT configuration if needed (their NAT settings are user-confirmed).
5. Verify actual connectivity and DNS resolution (all three IPv4 configuration baselines have been recorded).
6. Live domain controller discovery/authentication and name-resolution tests; both client memberships are verified via `Win32_ComputerSystem`.
7. Redacted screenshot evidence for architecture and configuration.

This is the configuration topology based on the information supplied to date. After live connectivity and DNS testing, add validated service paths.
