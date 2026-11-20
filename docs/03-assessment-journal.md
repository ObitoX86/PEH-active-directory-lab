# Assessment Journal

This log captures **observed** work, not just planned tutorials. Dates should reflect actual lab work when known.

## Prior activity — date to confirm

**Task:** Troubleshoot Windows file-sharing visibility from Punisher to the lab server.

**Observation:** Browsing through File Explorer's Network section did not initially show the expected server. Opening a direct UNC network path to the server's private IP succeeded.

**Interpretation:** Reachability via the direct server path was working even though network browsing/discovery did not initially expose the server as expected.

**Limits:** The share name, effective user permissions and final discovery configuration are not recorded here. Do not infer access beyond what was observed.

**Evidence:** Pending redacted screenshot or notes.

---

## 2026-11-20 — Server network configuration baseline

**Task:** Record the Windows Server network configuration using `ipconfig /all` in the existing VMware lab.

**Evidence:** Chat-supplied screenshot showing the command output (not yet uploaded to GitHub).

**Observed result:** Server reports host name `HYDRA-DC`, primary DNS suffix `MARVEL.local`, IPv4 `192.168.158.128`, subnet mask `255.255.255.0`, DHCP disabled, no configured default gateway, and DNS servers `::1` and `127.0.0.1`.

**Analysis:** This establishes a network-configuration baseline for the server VM and suggests local DNS resolution is intended. It does not establish the operational status of Active Directory Domain Services.

**Follow-up:** Verify AD DS role and the server's domain controller status; then inventory the client machines' IP/DNS settings.

---

## New entry template

**Date:** YYYY-MM-DD  
**Exercise:**  
**Target / authorised scope:**  
**Objective:**  
**Tools / commands:**  
**Evidence:**  
**Observed result:**  
**Analysis / limitations:**  
**Defensive mitigation:**  
**Follow-up:**
