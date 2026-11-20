# Network Architecture

## Observed machines

The VMware Workstation inventory screenshot (2026-11-20) contains three display names:

```text
VMware Workstation — My Computer
  - Windows Server 2022
  - THEPUNISHER
  - SPIDERMAN
```

The displayed names alone do not prove host roles, IP configuration, domain membership or network links.

## Verified connection from earlier troubleshooting

A Windows client referred to as **Punisher** accessed a lab Windows server using a direct UNC path by private IP. This is evidence of that specific connection; it does not verify all three VMware entries, the domain setup, or network discovery settings. The exact relationship between the previously reported client and **THEPUNISHER** is to be confirmed.

## Proposed diagram (pending verified topology)

```text
[Windows Server 2022]   [THEPUNISHER]   [SPIDERMAN]
         |                   |                |
  Network adapter, host role, IP and connectivity: TO VERIFY
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
