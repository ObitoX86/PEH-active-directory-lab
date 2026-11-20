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

**Interpretation:** DHCP is disabled; an IPv4 address is configured. The DNS servers are loopback addresses. An additional successful `Get-ADDomainController` query verifies the server is identified as an AD domain controller and Global Catalog; it does not independently test DNS availability or every AD service. The VMware virtual switch mode and which clients are on the same subnet are still unknown.

**Prior observation caveat:** An earlier troubleshooting session used a direct UNC path to `192.168.10.10`, whereas the current screenshot reports `192.168.158.128` for `HYDRA-DC`. Do not assume these are the same host/interface or that the network was changed without confirming the configuration.

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

**Comparison:** The client and HYDRA-DC (`192.168.158.128/24`) are configured in the same IPv4 subnet (`192.168.158.0/24`). The client's configured DNS address matches the domain controller's IPv4 address. This establishes intended DNS configuration, **not** a successful DNS lookup, verified domain membership, or end-to-end network connectivity. DHCP service is configured at a separate address (`192.168.158.254`). VMware network modes are still unverified.

## Verified connection from earlier troubleshooting

A Windows client referred to as **Punisher** accessed a lab Windows server using a direct UNC path by private IP. This is evidence of that specific connection; it does not verify all three VMware entries, the domain setup, or network discovery settings. The exact relationship between the previously reported client and **THEPUNISHER** is to be confirmed.

## Proposed diagram (pending verified topology)

```text
[Windows Server 2022 / HYDRA-DC]     [THEPUNISHER]       [SPIDERMAN]
   192.168.158.128/24               192.168.158.130/24       ?
      AD DC + GC                     DHCP client
           ^                               |
           +---------- DNS configured -----+
        192.168.158.0/24 (IP subnet for first two hosts)
          VMware network mode / reachability: TO VERIFY
```

No connections are drawn until their configuration is confirmed.

## Details to confirm

1. Installed operating systems and purposes for each of the three VMs.
2. Which additional domain controller services are functioning (AD controller identity is verified).
3. Additional domain configuration and DNS health (domain identified as `MARVEL.local`; publish only if safe).
4. VMware adapter types (NAT, host-only, custom VMnet, etc.).
5. SPIDERMAN IP address, subnet mask, gateway and DNS server settings (HYDRA-DC and THEPUNISHER baselines recorded).
6. Which hosts are domain-joined and how this was checked.
7. Redacted screenshot evidence for architecture and configuration.

Replace the draft with a diagram grounded in the running environment once confirmed.
