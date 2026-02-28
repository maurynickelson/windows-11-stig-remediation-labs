# WN10-00-000020  
## Secure Boot Must Be Enabled on Windows 11 Systems

**STIG ID:** WN10-00-000020  
**Severity:** High  
**System:** Windows 11 (Azure Virtual Machine)  
**Asset:** notengo  
**Assessment Tool:** Tenable / STIG Viewer  
**Assessment Date:** 02/01/2026  
**Analyst:** Maury Nickelson  

---

## Table of Contents

- [Skills Demonstrated](#skills-demonstrated)
- [Control Objective](#control-objective)
- [Security Risk](#security-risk)
- [Technical Background](#technical-background)
- [Phase 1 — Detection (Baseline Scan)](#phase-1--detection-baseline-scan)
- [Phase 2 — Validation & Analysis](#phase-2--validation--analysis)
- [Phase 3 — Remediation Decision](#phase-3--remediation-decision)
- [Phase 4 — Remediation Execution Constraints](#phase-4--remediation-execution-constraints)
- [Compensating Controls](#compensating-controls)
- [Recommended Remediation Path](#recommended-remediation-path)
- [Post-Validation Status](#post-validation-status)
- [Evidence](#evidence)
- [NIST 800-53 Mapping](#nist-800-53-mapping)
- [Compliance & Risk Position](#compliance--risk-position)

---

## Skills Demonstrated

- Secure Boot validation via PowerShell  
- UEFI vs Legacy BIOS analysis  
- Disk partition style verification (GPT vs MBR)  
- Cloud security architecture awareness (Azure VM limitations)  
- Risk-based remediation decision making  
- Compensating control documentation  
- High-severity vulnerability assessment  
- Enterprise risk acceptance workflow  
- NIST 800-53 mapping and control alignment  

---

## Control Objective

Ensure Secure Boot is enabled to enforce:

- Zero Trust at system boot  
- Defense in Depth  
- Pre-OS integrity validation  
- Protection against malicious bootloaders  

Secure Boot ensures only trusted firmware and bootloaders are executed during system startup.

---

## Security Risk

Without Secure Boot enabled:

- Malicious bootloaders may execute before antivirus, EDR, or logging systems
- Attack persistence becomes extremely difficult to detect
- Full operating system compromise can occur without user awareness
- Rootkits and bootkits may bypass OS-level protections

This control is rated **High severity** due to pre-OS attack exposure.

---

## Technical Background

Secure Boot requires:

- UEFI firmware (not Legacy BIOS)
- GPT partition style (not MBR)
- Platform-level enforcement
- For Azure VMs: **Generation 2 VM with Trusted Launch enabled at creation**

Secure Boot cannot be enabled post-deployment on certain Azure VM configurations.

---

# Phase 1 — Detection (Baseline Scan)

Initial Tenable STIG audit marked this control as **Failed**.

Secure Boot was reported as disabled.

---

# Phase 2 — Validation & Analysis

To validate the scanner finding:

```powershell
Confirm-SecureBootUEFI
```

### Result

Secure Boot returned **False**, confirming it is not enabled.

---

### Firmware Validation

Confirmed the system is running UEFI (not Legacy BIOS).

---

### Disk Partition Validation

Executed:

```powershell
Get-Disk | Select Number, IsBoot, IsSystem, PartitionStyle
```

### Result

- PartitionStyle = GPT  
- System supports Secure Boot  

This confirmed the system meets technical prerequisites.

The Tenable finding was validated as a **true positive**.

---

# Phase 3 — Remediation Decision

Because:

- The system supports UEFI  
- Disk is GPT  
- No legacy software constraints exist  

Secure Boot should be enabled.

However, additional platform analysis was required.

---

# Phase 4 — Remediation Execution Constraints

The system is hosted as an **Azure Virtual Machine**.

Secure Boot enforcement in Azure:

- Must be configured at VM creation
- Requires **Generation 2 VM**
- Requires **Trusted Launch enabled**
- Cannot be enabled retroactively

The existing VM was not deployed with Trusted Launch.

Therefore:

Secure Boot cannot be enabled without redeploying the VM.

This limitation is inherent to the Azure platform architecture.

---

# Compensating Controls

Although Secure Boot remains disabled, the system is protected by enterprise-level compensating controls:

- Azure Network Security Groups (NSGs)
- Endpoint Detection & Response (EDR)
- Full disk encryption
- Restricted administrative access
- Controlled inbound network exposure

These controls reduce exploitability but do not replace Secure Boot protection.

---

# Recommended Remediation Path

To achieve full compliance:

1. Redeploy VM as **Generation 2**
2. Enable **Trusted Launch**
3. Enable **Secure Boot**
4. Validate inside guest OS:

```powershell
Confirm-SecureBootUEFI
```

Expected output:

```
True
```

---

# Post-Validation Status

Secure Boot remains disabled.

Further remediation requires VM redeployment.

No in-place remediation is technically possible.

---

# Evidence

(Place screenshots and scan reports in `/evidence` folder)

Recommended:

- Baseline failure screenshot  
- PowerShell validation screenshot  
- [WN10-00-000020_Baseline_Tenable_Report.pdf](evidence/WN10-00-000020_Baseline_Tenable_Report.pdf)

---

# NIST 800-53 Mapping

| NIST Control | Control Name | Relevance |
|--------------|-------------|-----------|
| SI-7 | Software, Firmware, and Information Integrity | Ensures integrity of system boot process |
| CM-6 | Configuration Settings | Enforces secure firmware configuration |
| AC-6 | Least Privilege | Prevents unauthorized pre-OS execution |
| SC-28 | Protection of Information at Rest | Supports secure platform integrity |
| SI-2 | Flaw Remediation | Addresses platform-level security weakness |

---

# Compliance & Risk Position

Status: **Non-Compliant — Risk Accepted (Platform Limitation)**

Risk acceptance justified due to:

- Cloud architecture constraint
- Redeployment requirement
- Operational impact considerations

Mitigated via compensating security controls.

This case demonstrates practical enterprise decision-making, cloud architecture understanding, and risk management discipline.
