# Attack-to-Defence Mapping

These are **study topics from the PEH course**, not claims that this lab has reproduced the attacks.

| PEH topic | Lab verification | Defence / detection research needed |
| --- | --- | --- |
| LLMNR/NBT-NS poisoning | Not yet recorded | Name-resolution hardening; network monitoring |
| SMB relay | Not yet recorded | SMB signing and authentication configuration |
| IPv6 / DNS takeover | Not yet recorded | IPv6 policy, DHCPv6/DNS monitoring |
| Kerberoasting | **Prerequisite identified; no attack yet.** `SQLService` is a direct `Domain Admins` member with registered SPN `HYDRA-DC/SQLService.MARVEL.local:60111` (deliberate PEH lab setup) | Limit service-account privileges; prefer gMSAs where applicable; review SPN ownership, service-account password strength and Kerberos authentication monitoring |
| Pass-the-hash / pass-the-ticket | Not yet recorded | Credential protection and privilege boundaries |
| Credential dumping | Not yet recorded | Endpoint controls, least privilege and logging |
| Golden ticket concepts | Not yet recorded | Domain controller / KRBTGT security and recovery planning |

Add actual detection and mitigation evidence only after verifying the exercise and researching appropriate controls.
