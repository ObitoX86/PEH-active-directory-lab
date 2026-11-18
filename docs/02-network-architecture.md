# Network Architecture

## What is currently verified

During file-sharing troubleshooting, navigating directly to the lab Windows server by private IP from **Punisher** succeeded. This confirms a working client-to-server connection but does not by itself establish network discovery behaviour, domain membership or other services.

## Draft topology — roles only, not a verified wiring diagram

```text
        [Virtual lab network — verify mode/subnet]
                 /                  \
     [Domain controller]       [Punisher client]
                 \__________________/
                  Direct UNC path worked

[Other VMs, Kali testing VM, additional networks: to confirm]
```

## Details to confirm

1. Actual VM names and installed operating systems.
2. Domain name and domain settings (publish only if safe).
3. Network adapters (NAT, host-only, custom VMnet, etc.).
4. Subnets, DNS settings and domain controller role.
5. Which hosts were domain-joined and how this was checked.
6. Evidence screenshots with sensitive data redacted.

Replace the draft with a diagram grounded in the running environment once confirmed.
