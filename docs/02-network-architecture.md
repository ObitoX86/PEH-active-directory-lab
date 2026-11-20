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

**Interpretation:** DHCP is disabled; an IPv4 address is configured. The DNS servers are loopback addresses. The domain suffix and host name fit the intended lab design, but they do not verify that AD DS or DNS services are running. The VMware virtual switch mode and which clients are on the same subnet are still unknown.

**Prior observation caveat:** An earlier troubleshooting session used a direct UNC path to `192.168.10.10`, whereas the current screenshot reports `192.168.158.128` for `HYDRA-DC`. Do not assume these are the same host/interface or that the network was changed without confirming the configuration.

## Verified connection from earlier troubleshooting

A Windows client referred to as **Punisher** accessed a lab Windows server using a direct UNC path by private IP. This is evidence of that specific connection; it does not verify all three VMware entries, the domain setup, or network discovery settings. The exact relationship between the previously reported client and **THEPUNISHER** is to be confirmed.

## Proposed diagram (pending verified topology)

```text
[Windows Server 2022]         [THEPUNISHER]       [SPIDERMAN]
 host: HYDRA-DC                    ?                    ?
 192.168.158.128/24                ?                    ?
        Network type, client IPs and connectivity: TO VERIFY
```

No connections are drawn until their configuration is confirmed.

## Details to confirm

1. Installed operating systems and purposes for each of the three VMs.
2. Which machine runs Active Directory Domain Services.
3. Domain name and domain settings (publish only if safe).
4. VMware adapter types (NAT, host-only, custom VMnet, etc.).
5. IP addresses, subnet mask, gateway and DNS server settings.
6. Which hosts are domain-joined and how this was checked.
7. Redacted screenshot evidence for architecture and configuration.

Replace the draft with a diagram grounded in the running environment once confirmed.
