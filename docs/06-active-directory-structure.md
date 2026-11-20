# Active Directory Directory Structure

## Scope and evidence

The following organisational units were listed by the Active Directory PowerShell module on **HYDRA-DC** in the `MARVEL.local` domain, on **20 November 2026**.

```powershell
Get-ADOrganizationalUnit -Filter * |
    Select-Object Name, DistinguishedName
```

**Observed output:**

```text
Name                DistinguishedName
----                -----------------
Domain Controllers  OU=Domain Controllers,DC=MARVEL,DC=local
Groups              OU=Groups,DC=MARVEL,DC=local
```

## Directory structure observed

```text
MARVEL.local
├── OU=Domain Controllers
└── OU=Groups
```

The tree above shows **only organisational units returned by this command**. It is not a complete directory tree. Active Directory also contains other object classes and built-in containers (for example `CN=Users` and `CN=Computers`) that are **not included** in an `Get-ADOrganizationalUnit` listing.

| OU | Distinguished name | Interpretation |
| --- | --- | --- |
| Domain Controllers | `OU=Domain Controllers,DC=MARVEL,DC=local` | Standard OU commonly used to hold domain controller computer objects; membership not separately queried yet |
| Groups | `OU=Groups,DC=MARVEL,DC=local` | OU with 20 immediate child objects returned with `ObjectClass=group` |

### Notes for security analysis

- An **OU** is a directory container and is not itself a security group.
- A **distinguished name (DN)** identifies an object or container within the LDAP directory hierarchy.
- OUs can be targets of Group Policy links and delegated administration, but this output does not verify any GPO links or delegated permissions.
- The OU enumeration does not establish group memberships, permissions or privileges. A subsequent read-only query enumerates 20 group objects, but not their members or permissions.

## Groups OU — direct child objects verified on 2026-11-20

Command executed on HYDRA-DC:

```powershell
Get-ADObject -SearchBase "OU=Groups,DC=MARVEL,DC=local" -SearchScope OneLevel -Filter * |
    Select-Object Name, ObjectClass
```

**Observed output: 20 objects, each with `ObjectClass=group`.**

| # | Name | ObjectClass |
| ---: | --- | --- |
| 1 | Allowed RODC Password Replication Group | `group` |
| 2 | Cert Publishers | `group` |
| 3 | Cloneable Domain Controllers | `group` |
| 4 | Denied RODC Password Replication Group | `group` |
| 5 | DnsAdmins | `group` |
| 6 | DnsUpdateProxy | `group` |
| 7 | Domain Admins | `group` |
| 8 | Domain Computers | `group` |
| 9 | Domain Controllers | `group` |
| 10 | Domain Guests | `group` |
| 11 | Domain Users | `group` |
| 12 | Enterprise Admins | `group` |
| 13 | Enterprise Key Admins | `group` |
| 14 | Enterprise Read-only Domain Controllers | `group` |
| 15 | Group Policy Creator Owners | `group` |
| 16 | Key Admins | `group` |
| 17 | Protected Users | `group` |
| 18 | RAS and IAS Servers | `group` |
| 19 | Read-only Domain Controllers | `group` |
| 20 | Schema Admins | `group` |

**Interpretation:** All 20 are immediate child group objects in this OU. Their names include well-known privilege-relevant groups such as `Domain Admins`, `Enterprise Admins`, `Schema Admins` and `DnsAdmins`. The command doesn't establish group membership, permissions, GroupScope, GroupCategory or vulnerabilities. Built-in AD objects in other containers are outside this query's scope.

**Evidence:** Screenshot submitted in the conversation on 2026-11-20; redacted PNG not yet committed to the GitHub evidence folder.

## Next step — inspect direct membership of Domain Admins

```powershell
Get-ADGroupMember -Identity "Domain Admins" |
    Select-Object Name, ObjectClass
```

This is a read-only query listing direct members. It does not recursively expand nested group membership.

## Screenshot evidence

The original PowerShell screenshot was supplied in the PEH project conversation on 2026-11-20. A separately redacted PNG has **not yet been uploaded** to this repository. The output above is a transcription of the supplied evidence, not a new test.
