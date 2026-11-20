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
| Groups | `OU=Groups,DC=MARVEL,DC=local` | An OU named Groups; contents and whether it includes any group objects have not yet been verified |

### Notes for security analysis

- An **OU** is a directory container and is not itself a security group.
- A **distinguished name (DN)** identifies an object or container within the LDAP directory hierarchy.
- OUs can be targets of Group Policy links and delegated administration, but this output does not verify any GPO links or delegated permissions.
- The result does not establish the names, membership, permissions, or privileges of any domain security groups.

## Next verification

List the immediate child objects of the `Groups` OU using a read-only query:

```powershell
Get-ADObject -SearchBase "OU=Groups,DC=MARVEL,DC=local" -SearchScope OneLevel -Filter * |
    Select-Object Name, ObjectClass
```

This will identify which object classes, if any, are directly present in that OU. Empty output would mean no immediate child objects matched; it would not mean the domain has no groups.

## Screenshot evidence

The original PowerShell screenshot was supplied in the PEH project conversation on 2026-11-20. A separately redacted PNG has **not yet been uploaded** to this repository. The output above is a transcription of the supplied evidence, not a new test.
