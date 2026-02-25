# WN11-00-000065  
## Unused Accounts Must Be Disabled or Removed From the System After 35 Days of Inactivity

**STIG ID:** WN11-00-000065  
**Severity:** Medium  
**System:** Windows 11 (Local Account Policy)  
**Impacted Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Analyst:** Maury Nickelson  

---

## Skills Demonstrated

- Windows 11 account lifecycle management  
- Local user enumeration via PowerShell  
- Inactive account detection logic  
- SID analysis and built-in account identification  
- Account risk assessment  
- Secure account disablement  
- Vulnerability scan validation workflow  
- Least privilege enforcement  
- Identity & Access Management (IAM) alignment  
- Compliance validation through re-scan  

---

## Control Objective

Ensure unused local accounts are disabled or removed after 35 days of inactivity.

This control reduces exposure from dormant accounts that may be leveraged for:

- Unauthorized access  
- Privilege escalation  
- Lateral movement  
- Persistence mechanisms  

---

## Security Risk

Inactive local accounts increase attack surface by providing unused credentials that may:

- Be targeted by brute force attacks  
- Be reused by unauthorized individuals  
- Remain unnoticed during security monitoring  

This control enforces:

- Least privilege  
- Proper Identity & Access Management (IAM)  
- Account lifecycle governance  

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable vulnerability scan identified the system as **Non-Compliant**.

### Evidence of Non-Compliance

Scan detected enabled local account(s) with no logon activity in over 35 days.

Identified account:

- Administrator  

---

# Phase 2 — Validation & Analysis

Before remediating, I validated the scan finding to ensure it was not a false positive.

Executed the following PowerShell command:

```powershell
Get-LocalUser |
Where-Object { $_.Enabled -eq $true -and $_.LastLogon -lt (Get-Date).AddDays(-35) }
```

### Validation Result

The command returned:

- Administrator (Enabled = True)

The `LastLogon` field returned no value, indicating:

- The account had no recorded interactive logon
- Or had never been used

This confirmed the scan finding was a **true positive**.

---

## Account Identification & Risk Analysis

To determine whether the account was a built-in system administrator or manually created, I executed:

```powershell
Get-LocalUser | Select-Object Name, SID, Enabled
```

### Findings

- SID ended in **-1000**
- Built-in Administrator accounts end in **-500**

This confirmed:

- The account was **not** a built-in system-critical administrator  
- It was manually created  
- It was safe to disable  

This analysis ensured remediation would not impact system integrity.

---

# Phase 3 — Remediation

Because the account was unused and non-critical, remediation was required.

Rather than deleting the account, I chose to **disable** it to preserve recoverability if needed.

Executed:

```powershell
Disable-LocalUser -Name "Administrator"
```

This prevented the account from being used while maintaining forensic traceability.

---

# Phase 4 — Post-Remediation Validation

To confirm the account was successfully disabled:

```powershell
Get-LocalUser -Name "Administrator"
```

Verified:

- Enabled = False

Additionally, the system was rescanned using Tenable.

### Scan Result

The control returned a **Warning** status rather than “Passed” due to the presence of the built-in Administrator account still enabled, which may require additional policy-level configuration.

However, the manually created inactive account was successfully remediated.

---

## Evidence

(Place your screenshots and PDFs in the `evidence/` folder and reference them here)

Recommended structure:

- Baseline scan screenshot  
- Post-remediation scan screenshot  
- [Baseline Scan Report (PDF)](evidence/baseline_scan.pdf)  
- [Post-Remediation Scan Report (PDF)](evidence/post_remediation_scan.pdf)  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| AC-2 | Account Management | Governs account lifecycle and inactivity policies |
| AC-6 | Least Privilege | Prevents unnecessary account access |
| IA-2 | Identification & Authentication | Ensures only active authorized accounts are usable |
| IA-4 | Identifier Management | Controls account creation and maintenance |
| CM-6 | Configuration Settings | Enforces account configuration compliance |

---

## Compliance Impact

This remediation:

- Reduced attack surface  
- Strengthened local account governance  
- Enforced least privilege  
- Improved audit readiness  
- Demonstrated vulnerability validation discipline  
