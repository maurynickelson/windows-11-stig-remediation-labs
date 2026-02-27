# WN11-CC-000110  
## Printing over HTTP must be prevented

**STIG ID:** WN11-CC-000110  
**Severity:** Medium  
**System:** Windows 11  
**Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Assessment Date:** 01/29/2026  
**Analyst:** Maury Nickelson  

---

## Skills Demonstrated

- Windows 11 STIG remediation  
- Registry policy validation via PowerShell  
- Print Spooler attack surface reduction  
- True-positive vulnerability confirmation  
- Policy-based remediation enforcement  
- Secure configuration management  
- Compliance verification via re-scan  
- NIST 800-53 control alignment  

---

## Control Objective

Prevent printing over HTTP by ensuring the policy `DisableHTTPPrinting` is enforced.

Printing over HTTP increases exposure of the Print Spooler service and may introduce unnecessary risk in secure environments.

---

## Security Risk

Allowing HTTP printing can:

- Increase Print Spooler attack surface  
- Enable remote code execution pathways (if combined with spooler vulnerabilities)  
- Facilitate privilege escalation  
- Support lateral movement  

Disabling HTTP printing strengthens system hardening and reduces exposure.

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked the control as **Failed**.

The vulnerability scan identified that HTTP printing was enabled.

---

# Phase 2 — Validation & Analysis

Before remediation, I validated the scan finding to confirm it was not a false positive.

Executed:

```powershell
Get-ItemProperty `
  -Path "HKLM:\Software\Policies\Microsoft\Windows NT\Printers" `
  -Name DisableHTTPPrinting `
  -ErrorAction SilentlyContinue
```

### Validation Result

The registry value returned:

```
DisableHTTPPrinting : 0
```

A value of **0** indicates HTTP printing is enabled.

This confirmed the Tenable finding was a **true positive**.

---

# Phase 3 — Remediation

Because HTTP printing increases potential exposure, remediation was required.

## Remediation Execution

Executed:

```powershell
Set-ItemProperty `
  -Path "HKLM:\Software\Policies\Microsoft\Windows NT\Printers" `
  -Name DisableHTTPPrinting `
  -Type DWord `
  -Value 1
```

Setting the value to **1** enforces the policy and disables HTTP printing.

This change aligns the system with STIG hardening requirements.

---

# Phase 4 — Post-Remediation Validation

To confirm successful remediation:

```powershell
Get-ItemProperty `
  -Path "HKLM:\Software\Policies\Microsoft\Windows NT\Printers" `
  -Name DisableHTTPPrinting
```

### Validation Result

```
DisableHTTPPrinting : 1
```

A value of **1** confirms HTTP printing is disabled.

Additionally, the Tenable scan was re-run to verify compliance.

### Post-Remediation Result

Control WN11-CC-000110 returned a compliant status.

---

## Evidence

(Place screenshots and reports inside the `evidence/` folder and reference them below)

Recommended structure:

- Baseline audit screenshot (failed status)  
- Post-remediation audit screenshot (passed status)  
- [Baseline Scan Report (PDF)](evidence/baseline_scan.pdf)  
- [Post-Remediation Scan Report (PDF)](evidence/post_remediation_scan.pdf)  

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| CM-6 | Configuration Settings | Enforces secure system configuration |
| AC-3 | Access Enforcement | Restricts unintended network printing behavior |
| AC-6 | Least Privilege | Reduces unnecessary service exposure |
| SI-2 | Flaw Remediation | Addresses configuration weaknesses |
| SC-7 | Boundary Protection | Limits insecure communication channels |

---

## Compliance Impact

This remediation:

- Reduced Print Spooler attack surface  
- Eliminated unnecessary HTTP-based printing exposure  
- Strengthened configuration hardening posture  
- Demonstrated registry-level policy enforcement  
- Validated compliance through scanner confirmation  
