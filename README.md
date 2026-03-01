# 🔐 Windows 11 STIG Remediation Labs  
Enterprise Endpoint Hardening | Vulnerability Management | Cloud Security Boundary Analysis  

**Author:** Maury Nickelson  
**Platform:** Windows 11 (Local + Azure Virtual Machine)  
**Assessment Tool:** Tenable Vulnerability Management / STIG Viewer  
**Framework Alignment:** DISA STIG | NIST 800-53  

---

# 📌 Executive Overview

This repository documents the remediation lifecycle of ten DISA Windows 11 STIG controls across endpoint and cloud-hosted environments.

Each control follows a structured security operations methodology:

> Detect → Validate → Remediate → Verify → Document → Map to NIST

This project demonstrates practical experience in:

- Windows endpoint hardening  
- Registry and Group Policy security enforcement  
- Network stack security configuration  
- Boot-level trust validation  
- Vulnerability scanner validation and remediation  
- Cloud infrastructure boundary analysis  
- Risk acceptance and compensating control documentation  

All controls were validated using Tenable re-scans and manual PowerShell verification.

---

# 📂 Table of Contents (STIG Control Index)

| STIG ID | Control Title | Severity | Lab Folder |
|----------|--------------|----------|------------|
| WN11-UR-000025 | Allow Log On Locally Restriction | Medium | [View Lab](./WN11-UR-000025) |
| WN11-00-000065 | Unused Accounts Must Be Disabled After 35 Days | Medium | [View Lab](./WN11-00-000065) |
| WN11-CC-000110 | Printing Over HTTP Must Be Prevented | Medium | [View Lab](./WN11-CC-000110) |
| WN11-00-000135 | Host-Based Firewall Must Be Enabled | High | [View Lab](./WN11-00-000135) |
| WN11-SO-000025 | Built-In Guest Account Must Be Renamed | Medium | [View Lab](./WN11-SO-000025) |
| WN11-00-000045 | Antivirus Must Be Installed and Enabled | High | [View Lab](./WN11-00-000045) |
| WN11-CC-000197 | Microsoft Consumer Experiences Disabled | Low | [View Lab](./WN11-CC-000197) |
| WN11-CC-000391 | Internet Explorer Must Be Disabled | High | [View Lab](./WN11-CC-000391) |
| WN11-CC-000020 | IPv6 Source Routing Highest Protection | Medium | [View Lab](./WN11-CC-000020) |
| WN11-00-000020 | Secure Boot Must Be Enabled | High | [View Lab](./WN11-00-000020) |

---

# 📊 Control Coverage Matrix

| STIG ID | Endpoint Hardening | Network Security | Boot Integrity | Registry/GPO Enforcement | Vulnerability Validation | Cloud Awareness |
|----------|------------------|------------------|---------------|--------------------------|--------------------------|-----------------|
| WN11-UR-000025 | ✔ |  |  | ✔ | ✔ |  |
| WN11-00-000065 | ✔ |  |  | ✔ | ✔ |  |
| WN11-CC-000110 | ✔ | ✔ |  | ✔ | ✔ |  |
| WN11-00-000135 | ✔ | ✔ |  | ✔ | ✔ |  |
| WN11-SO-000025 | ✔ |  |  | ✔ | ✔ |  |
| WN11-00-000045 | ✔ |  |  | ✔ | ✔ |  |
| WN11-CC-000197 | ✔ |  |  | ✔ | ✔ |  |
| WN11-CC-000391 | ✔ | ✔ |  | ✔ | ✔ |  |
| WN11-CC-000020 | ✔ | ✔ |  | ✔ | ✔ |  |
| WN11-00-000020 | ✔ |  | ✔ |  | ✔ | ✔ |

---

# 🧠 NIST 800-53 Mapping Overview

| STIG ID | Primary NIST Controls |
|----------|----------------------|
| WN11-UR-000025 | AC-2, AC-3, AC-6 |
| WN11-00-000065 | AC-2, IA-5 |
| WN11-CC-000110 | CM-6, AC-3, SC-7 |
| WN11-00-000135 | SC-7, AC-4, CM-6 |
| WN11-SO-000025 | AC-2, IA-5 |
| WN11-00-000045 | SI-3, SI-2, CM-6 |
| WN11-CC-000197 | CM-6, CM-7 |
| WN11-CC-000391 | CM-7, SI-2, SC-7 |
| WN11-CC-000020 | SC-7, AC-4, CM-6 |
| WN11-00-000020 | SI-7, CM-6, SC-7 |

---

# 📈 Severity Breakdown

| Severity | Count |
|----------|-------|
| High | 4 |
| Medium | 5 |
| Low | 1 |

High-Severity Controls:
- Host-Based Firewall Enforcement
- Antivirus Enforcement
- Internet Explorer Removal
- Secure Boot Validation (Cloud Infrastructure Constraint)

---

# 🛠 Core Technical Competencies Demonstrated

## Endpoint Security & Hardening
- Windows registry security configuration
- Group Policy enforcement and validation
- User rights assignment restriction
- Local account lifecycle management
- Secure configuration baseline alignment

## Network & Protocol Security
- IPv6 routing protection
- Host-based firewall configuration
- HTTP printing exposure mitigation
- Boundary protection enforcement
- Network attack surface reduction

## Boot & Platform Integrity
- Secure Boot validation
- UEFI vs BIOS architecture analysis
- GPT disk validation
- Hardware-rooted trust verification

## Vulnerability Management
- Tenable STIG audit interpretation
- True-positive validation
- Scanner discrepancy analysis
- Remediation verification via re-scan
- Security evidence documentation

## Cloud Security Awareness
- Azure VM Generation analysis
- Trusted Launch architectural constraints
- Infrastructure vs guest OS control boundaries
- Risk acceptance documentation
- Compensating control assessment

---

# 🔄 Remediation Lifecycle Methodology

Each STIG control followed a standardized vulnerability management process:

1. Baseline detection via Tenable STIG audit  
2. Manual validation using PowerShell / Registry / Group Policy  
3. True-positive confirmation  
4. Secure remediation implementation  
5. Post-remediation validation  
6. Evidence collection and documentation  
7. NIST 800-53 control alignment  

This mirrors real-world SOC and vulnerability management workflows.

---

# 🛡 Security Impact Summary

This project demonstrates the ability to:

- Harden Windows endpoints to DISA STIG standards  
- Reduce attack surface across OS, network, and boot layers  
- Validate vulnerability scanner findings with technical analysis  
- Enforce secure configuration baselines  
- Handle high-severity vulnerabilities responsibly  
- Identify infrastructure-level control limitations  
- Document compensating controls and risk acceptance  

---

# 🎯 Role Alignment

This repository directly aligns with responsibilities in:

- SOC Analyst  
- Vulnerability Management Analyst  
- Endpoint Security Analyst  
- Cloud Security Analyst  
- Security Operations Engineer  

Core skill keywords:

Windows hardening • PowerShell • Registry auditing • Group Policy • Tenable • Vulnerability remediation • Endpoint security • Network security • Secure configuration management • Azure VM security • Security baseline enforcement • NIST 800-53 alignment • Risk documentation

---

# 🚀 Portfolio Significance

This repository represents structured, hands-on security engineering work demonstrating:

- Technical depth  
- Process discipline  
- Framework alignment  
- Real-world remediation logic  
- Cloud and endpoint security integration  

It reflects applied security operations experience aligned with enterprise security standards.
