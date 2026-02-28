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
- PowerShell user enumeration  
- Local security option configuration  
- OS-level account hardening  
- Defense-in-depth implementation  
- Vulnerability scan validation workflow  
- Least privilege reinforcement  

---

## Control Objective

Ensure the built-in Guest account is renamed to a non-default value.

This control is implemented at the operating system level and reduces predictability of default accounts.

---

## Security Risk

The built-in Guest account:

- Is disabled by default  
- Has a well-known SID ending in `-501`  
- Has a predictable default name (“Guest”)  

Even though the account is disabled, retaining the default name increases predictability for attackers performing enumeration.

Renaming the account:

- Reduces account enumeration effectiveness  
- Supports defense-in-depth  
- Strengthens baseline hardening posture  

Severity: **Low**

---

## Technical Background

Built-in Guest account characteristics:

- SID ends in `-501`
- Disabled by default
- Renameable via:
  - Local Security Policy
  - PowerShell
  - Group Policy

To identify the built-in Guest account:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked this control as **Failed**.

The Guest account retained the default name “Guest”.

---

# Phase 2 — Validation & Analysis

Validated the scan finding using PowerShell:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

### Validation Result (Pre-Remediation)

- Name: Guest  
- Enabled: False  
- SID ending: -501  

Confirmed:

- Account is disabled  
- Account name is still “Guest”  

This validated the Tenable finding as a **true positive**.

---

# Phase 3 — Remediation

Remediation required renaming the built-in Guest account.

## Option 1 — Local Security Policy

Navigate to:

Computer Configuration →  
Windows Settings →  
Security Settings →  
Local Policies →  
Security Options →  
**Accounts: Rename guest account**

Set value to a non-default name.

---

## Option 2 — PowerShell (Used)

```powershell
$guest = Get-LocalUser | Where-Object {$_.SID -like "*-501"}
Rename-LocalUser -Name $guest.Name -NewName "DisabledGuest_01"
```

This method:

- Dynamically identifies the built-in Guest account via SID  
- Renames it regardless of current name  
- Ensures accuracy even if previously renamed  

---

# Phase 4 — Post-Remediation Validation

Re-ran validation command:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
```

### Validation Result (Post-Remediation)

- Name: DisabledGuest_01  
- Enabled: False  

Renaming successful.

Tenable re-scan confirmed the control passed.

---

# Evidence

(Place artifacts inside `/evidence` folder)

Recommended:

- Baseline failed audit screenshot  
- Pre-remediation PowerShell output  
- Post-remediation PowerShell output  
- WN11-SO-000025_Baseline_Tenable_Report.pdf  
- WN11-SO-000025_Post-Remediation_Tenable_Report.pdf  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| AC-2 | Account Management | Governs account configuration and lifecycle |
| AC-6 | Least Privilege | Reduces predictable account exposure |
| IA-2 | Identification & Authentication | Supports secure account management |
| CM-6 | Configuration Settings | Enforces system-level security settings |

---

# Compliance & Security Impact

This remediation:

- Reduced account name predictability  
- Strengthened OS-level hardening posture  
- Reinforced defense-in-depth principles  
- Demonstrated proper identification of built-in system accounts  
- Validated compliance through re-scan  

While low severity, this control contributes to overall secure configuration hygiene and layered defense strategy.
