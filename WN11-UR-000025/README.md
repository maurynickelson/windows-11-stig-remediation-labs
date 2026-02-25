# WN11-UR-000025  
## The "Allow log on locally" user right must only be assigned to the Administrators and Users groups

**STIG ID:** WN11-UR-000025  
**Severity:** Medium  
**System:** Windows 11 Pro Build 26200  
**Host:** notengo  
**Assessment Tool:** Tenable Vulnerability Management  
**Assessment Date:** 05 Feb 2026  
**Analyst:** Maury Nickelson  

---

## Control Objective

Restrict interactive logon rights (SeInteractiveLogonRight) to only:

- Administrators  
- Users  

This prevents unauthorized accounts or groups from logging on locally to the system.

---

## Security Risk

If additional accounts are granted interactive logon rights, risk increases for:

- Unauthorized local access  
- Privilege escalation  
- Persistence  
- Lateral movement staging  

This control enforces least privilege and reduces the attack surface.

---

# Baseline Scan Results

Initial Tenable scan identified the system as **Non-Compliant**.

Manual validation was performed using:

```powershell
secedit /export /cfg C:\temp\secpol.cfg
Select-String "SeInteractiveLogonRight" C:\temp\secpol.cfg
```

### Baseline Finding

The following accounts/groups had local logon rights:

- Administrators  
- Users  
- DisabledGuest01  
- Backup Operators  

This confirmed a **true positive** finding.

---

# Remediation Process

## Step 1 – Create Security Template

```powershell
@"
[Unicode]
Unicode=yes
[Version]
signature="$CHICAGO$"
Revision=1
[Privilege Rights]
SeInteractiveLogonRight = *S-1-5-32-544,*S-1-5-32-545
"@ | Set-Content -Path "C:\temp\WN11-UR-000025.inf" -Encoding Unicode
```

## Step 2 – Apply Template

```powershell
secedit /configure /db C:\temp\secedit.sdb /cfg C:\temp\WN11-UR-000025.inf /areas USER_RIGHTS
gpupdate /force
```

---

## Issue Encountered

Setting did not change after applying template.

Root Cause:

Local Group Policy was enforcing the user right and overriding local security template changes.

---

# Final Remediation

Modified setting via Local Group Policy:

Computer Configuration →  
Windows Settings →  
Security Settings →  
Local Policies →  
User Rights Assignment →  
Allow log on locally  

Configured to include only:

- Administrators  
- Users  

---

# Post-Remediation Validation

Re-exported security configuration:

```powershell
secedit /export /cfg C:\temp\secpol.cfg
Select-String "SeInteractiveLogonRight" C:\temp\secpol.cfg
```

Re-ran Tenable scan.

## Tenable Scan Summary (Post-Remediation)

- Host: notengo  
- OS: Windows 11 25H2  
- Scan Date: 05 Feb 2026  
- Critical: 0  
- High: 1  
- Medium: 4  
- Low: 0  
- Informational: 94  
- Total Findings: 99  

Control WN11-UR-000025 passed compliance validation.

---

## Evidence

- [Baseline Scan Report](evidence/baseline_scan.pdf)
- [Post-Remediation Scan Report](evidence/post_remediation_scan.pdf)

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| AC-2 | Account Management | Ensures only authorized accounts are assigned logon rights |
| AC-3 | Access Enforcement | Enforces system-level access control restrictions |
| AC-5 | Separation of Duties | Prevents inappropriate privilege overlap |
| AC-6 | Least Privilege | Restricts interactive access to required groups only |
| IA-2 | Identification & Authentication | Supports controlled interactive session authentication |

---

# Skills Demonstrated

- Windows 11 STIG remediation  
- True positive validation  
- Security template enforcement  
- Group Policy troubleshooting  
- Compliance verification via Tenable  
- Security configuration auditing with secedit  
- Policy-level remediation validation  
- NIST control mapping documentation  
