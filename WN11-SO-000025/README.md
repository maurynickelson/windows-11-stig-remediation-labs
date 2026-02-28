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
- OS-level account hardening (rename security option)  
- Defense-in-depth implementation  
- Vulnerability scan validation workflow  

---

## Control Objective

Ensure the built-in Guest account is renamed to a non-default value.

This control is implemented at the operating system level and reduces predictability of default accounts.

---

## Security Risk

The built-in Guest account is:

- Disabled by default  
- Identifiable via a well-known SID ending in `-501`  
- Predictable when named “Guest”  

Even though the account is disabled, retaining the default name increases predictability for attackers performing enumeration. Renaming the account supports defense-in-depth and hardening hygiene.

---

## Technical Background

The built-in Guest account can be identified by its SID ending in `-501`.

Validation command used:

```powershell
Get-LocalUser | Where-Object {$_.SID -like "*-501"} | Select Name, Enabled, SID
