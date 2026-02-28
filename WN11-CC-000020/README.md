# WN11-CC-000020  
## IPv6 Source Routing Must Be Configured to Highest Protection

**STIG ID:** WN11-CC-000020  
**Severity:** Medium  
**System:** Windows 11  
**Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Assessment Date:** 02/08/2026  
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

- Windows network stack hardening  
- IPv6 configuration security enforcement  
- Registry-based TCP/IP parameter auditing  
- PowerShell registry interrogation and modification  
- Network attack surface reduction  
- Vulnerability remediation lifecycle (detect → validate → remediate → verify)  
- Security baseline alignment with STIG controls  
- Medium-severity vulnerability handling and documentation  

---

## Control Objective

Configure IPv6 source routing to the highest protection level by disabling it.

This control enforces system-level network hardening to prevent attackers from manipulating packet routing paths to bypass security controls or monitoring systems.

Required configuration:

```
DisableIPSourceRouting = 2
```

---

## Security Risk

If IPv6 source routing is enabled or not set to highest protection:

- Attackers may manipulate packet routing paths  
- Security monitoring controls may be bypassed  
- Filtering mechanisms may be evaded  
- Network reconnaissance may be facilitated  
- Lateral movement risk increases  

Severity: **Medium**

Disabling IPv6 source routing reduces routing manipulation risks and strengthens network-level defenses.

---

## Technical Background

Registry path:

```
HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters
```

Registry value:

```
DisableIPSourceRouting
```

Value meanings:

- 0 = No protection  
- 1 = Medium protection  
- 2 = Highest protection (required)

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked this control as **Failed**.

The system was not configured to highest IPv6 source routing protection.

---

# Phase 2 — Validation & Analysis

Manual validation was performed using PowerShell:

```powershell
Get-ItemProperty `
  -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" `
  -Name DisableIPSourceRouting `
  -ErrorAction SilentlyContinue
```

### Validation Result (Pre-Remediation)

Command returned:

- No output  
  OR  
- Value not equal to `2`

This confirmed the Tenable finding was a **true positive**.

IPv6 source routing was not configured to highest protection.

---

# Phase 3 — Remediation

Remediation required configuring the registry value to `2`.

Executed:

```powershell
New-ItemProperty `
  -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" `
  -Name "DisableIPSourceRouting" `
  -PropertyType DWord `
  -Value 2 `
  -Force
```

This ensured:

```
DisableIPSourceRouting = 2
```

System was rebooted to ensure configuration enforcement.

Policy refresh executed:

```powershell
gpupdate /force
```

---

# Phase 4 — Post-Remediation Validation

Re-validated registry configuration:

```powershell
Get-ItemProperty `
  -Path "HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip6\Parameters" `
  -Name DisableIPSourceRouting
```

### Validation Result

```
DisableIPSourceRouting : 2
```

This confirms highest protection level is enforced.

Tenable re-scan confirmed the control passed.

---

# Evidence

(Place artifacts inside `/evidence` folder)

Recommended filenames:

- WN11-CC-000020_Baseline_Failed_Audit.png  
- WN11-CC-000020_Pre_Remediation_Registry_Check.png  
- WN11-CC-000020_Post_Remediation_Registry_Check.png  
- WN11-CC-000020_Post_Remediation_Passed_Audit.png  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| SC-7 | Boundary Protection | Prevents routing manipulation attacks |
| CM-6 | Configuration Settings | Enforces secure network configuration |
| AC-4 | Information Flow Enforcement | Controls packet routing behavior |
| SI-2 | Flaw Remediation | Addresses configuration weaknesses |

---

# Compliance & Security Impact

This remediation:

- Eliminated IPv6 routing manipulation exposure  
- Strengthened TCP/IP configuration baseline  
- Reduced network attack surface  
- Enforced secure routing configuration  
- Demonstrated structured remediation lifecycle execution  

This control enhances network-layer security posture and mitigates routing-based evasion techniques.
