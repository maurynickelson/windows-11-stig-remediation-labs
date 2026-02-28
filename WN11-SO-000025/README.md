# WN11-SO-000025  
## The Built-In Guest Account Must Be Renamed

**STIG ID:** WN11-SO-000025  
**Severity:** Low  
**System:** Windows 11  
**Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Assessment Date:** 02/03/2026  
**Analyst:** Maury Nickelson  

---

## Table of Contents

- [Skills Demonstrated](#skills-demonstrated)
- [Control Objective](#control-objective)
- [Security Risk](#security-risk)
- [Technical Background](#technical-background)
- [Phase 1 — Detection (Baseline Scan)](#phase-1--detection-baseline-scan)
- [Phase 2 — Validation & Analysis](#phase-2--validation--analysis)
- [Phase 3 — Remediation](#phase-3--remediation)
- [Phase 4 — Post-Remediation Validation](#phase-4--post-remediation-validation)
- [Evidence](#evidence)
- [NIST 800-53 Mapping](#nist-800-53-mapping)
- [Compliance & Security Impact](#compliance--security-impact)

---

## Skills Demonstrated

- Built-in account identification via SID  
- PowerShell user enumeration and validation  
- Local security option configuration  
- Defense-in-depth implementation  
- Secure baseline enforcement  
- Vulnerability validation workflow  
- Post-remediation verification discipline  

---

## Control Objective

Ensure the built-in Guest account is renamed to a non-default value.

This STIG is a technical configuration control enforced at the operating system level. It reduces predictability of well-known built-in accounts and strengthens system hardening.

---

## Security Risk

Although the built-in Guest account is disabled by default, it:

- Has a well-known SID ending in `-501`
- Has a predictable default name (“Guest”)
- Can be targeted during automated account enumeration

Renaming the account:

- Reduces attack predictability  
- Strengthens baseline configuration hygiene  
- Supports defense-in-depth  

Severity: **Low**

---

## Technical Background

The built-in Guest account can be uniquely identified by its SID:

```
*-501
```

Validation command used:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

This ensures remediation targets the actual built-in account regardless of its current name.

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked this control as **Failed**.

The Guest account retained the default name “Guest.”

### Baseline Audit Status

![Baseline Failed Audit](evidence/baseline_failed_audit.png)

---

# Phase 2 — Validation & Analysis

To confirm the scan finding was not a false positive, I validated using PowerShell:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

### Pre-Remediation PowerShell Output

![Pre-Remediation PowerShell Output](evidence/pre_remediation_powershell_output.png)

Findings:

- Name: Guest  
- Enabled: False  
- SID ending in -501  

This confirmed:

- The account is disabled  
- The name was still the default “Guest”  
- The Tenable finding was a **true positive**

---

# Phase 3 — Remediation

Remediation required renaming the built-in Guest account.

## Option 1 — Local Security Policy (GUI)

Navigate to:

Computer Configuration →  
Windows Settings →  
Security Settings →  
Local Policies →  
Security Options →  
**Accounts: Rename guest account**

Set to a value other than “Guest.”

---

## Option 2 — PowerShell (Used)

To ensure the correct account was targeted regardless of name, remediation was performed using SID-based identification:

```powershell
$guest = Get-LocalUser | Where-Object {$_.SID -like "*-501"}
Rename-LocalUser -Name $guest.Name -NewName "DisabledGuest_01"
```

This approach:

- Dynamically identifies the built-in Guest account  
- Ensures correct targeting  
- Prevents accidental renaming of other accounts  

---

# Phase 4 — Post-Remediation Validation

Validation was performed using the same SID-based query:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

### Post-Remediation PowerShell Output

![Post-Remediation PowerShell Output](evidence/post_remediation_powershell_output.png)

Result:

- Name: DisabledGuest_01  
- Enabled: False  

Renaming was successful.

Tenable re-scan confirmed compliance.

### Post-Remediation Scan Result

![Post-Remediation Scan](evidence/post_remediation_scan.png)

---

# Evidence

Artifacts stored in `/evidence`:

- `baseline_failed_audit.png`  
- `pre_remediation_powershell_output.png`  
- `post_remediation_powershell_output.png`  
- `post_remediation_scan.png`  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| AC-2 | Account Management | Governs system account configuration |
| AC-6 | Least Privilege | Reduces predictable exposure paths |
| IA-2 | Identification & Authentication | Supports secure account administration |
| CM-6 | Configuration Settings | Enforces secure system configuration baseline |

---

# Compliance & Security Impact

This remediation:

- Reduced account name predictability  
- Strengthened OS-level hardening posture  
- Demonstrated correct identification of built-in accounts via SID  
- Reinforced defense-in-depth practices  
- Validated remediation through post-scan confirmation  

Although low severity, this control contributes to overall configuration integrity and layered security posture.
