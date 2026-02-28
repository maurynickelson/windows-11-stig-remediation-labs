# WN11-CC-000197  
## Microsoft Consumer Experiences Must Be Turned Off

**STIG ID:** WN11-CC-000197  
**Severity:** Low  
**System:** Windows 11  
**Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Assessment Date:** 02/06/2026  
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

- Windows Group Policy validation  
- Registry-based security control enforcement  
- PowerShell registry interrogation  
- Secure configuration baseline alignment  
- Attack surface reduction through policy hardening  
- Vulnerability remediation lifecycle documentation  
- STIG compliance validation  

---

## Control Objective

Prevent Microsoft Consumer Experience features from being enabled on Windows 11 systems.

This control enforces a Group Policy setting that disables:

- Automatic installation of consumer apps  
- Unsolicited application suggestions  
- Consumer-targeted features  

The objective is to maintain a hardened enterprise baseline.

---

## Security Risk

Although low severity, leaving Microsoft consumer experiences enabled may:

- Introduce unnecessary applications  
- Increase system attack surface  
- Install non-essential software components  
- Reduce configuration standardization  

While not directly exploitable, disabling these features strengthens system hygiene and reduces unnecessary exposure.

Severity: **Low**

---

## Technical Background

The Group Policy setting:

**Turn off Microsoft consumer experiences**

Is enforced via registry key:

```
HKLM:\SOFTWARE\Policies\Microsoft\Windows\CloudContent
```

Registry value:

```
DisableWindowsConsumerFeatures
```

Required value:

```
1
```

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked this control as **Failed**.

The system did not have the required policy configured.

---

# Phase 2 — Validation & Analysis

To confirm the finding was accurate, PowerShell validation was performed:

```powershell
Get-ItemProperty `
  -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CloudContent" `
  -Name DisableWindowsConsumerFeatures `
  -ErrorAction SilentlyContinue
```

### Validation Result (Pre-Remediation)

The policy was:

- Not configured  
- Or set to 0  

This confirmed the Tenable finding was a **true positive**.

---

# Phase 3 — Remediation

Remediation required configuring the registry policy value to enforce the Group Policy setting.

Executed:

```powershell
New-ItemProperty `
  -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CloudContent" `
  -Name "DisableWindowsConsumerFeatures" `
  -PropertyType DWord `
  -Value 1 `
  -Force
```

This sets:

```
DisableWindowsConsumerFeatures = 1
```

Which disables Microsoft consumer experience features.

---

# Phase 4 — Post-Remediation Validation

Validation was performed using:

```powershell
Get-ItemProperty `
  -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows\CloudContent" `
  -Name DisableWindowsConsumerFeatures
```

### Validation Result

```
DisableWindowsConsumerFeatures : 1
```

This confirms:

- Group Policy is enforced  
- Consumer experience features are disabled  

A Tenable re-scan confirmed the control passed.

---

# Evidence

(Place artifacts in `/evidence` folder)

Recommended filenames:

- WN11-CC-000197_Baseline_Failed_Audit.png  
- WN11-CC-000197_Pre_Remediation_Registry_Check.png  
- WN11-CC-000197_Post_Remediation_Registry_Check.png  
- WN11-CC-000197_Post_Remediation_Passed_Audit.png  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| CM-6 | Configuration Settings | Enforces secure system configuration baseline |
| CM-7 | Least Functionality | Reduces unnecessary features and services |
| AC-6 | Least Privilege | Supports principle of minimal exposure |
| SI-2 | Flaw Remediation | Addresses configuration weakness |

---

# Compliance & Security Impact

This remediation:

- Reduced unnecessary feature exposure  
- Strengthened configuration standardization  
- Aligned system with secure enterprise baseline  
- Demonstrated registry-based policy enforcement  
- Validated compliance through scanner confirmation  

Although low severity, this control supports a hardened and standardized Windows 11 environment.
