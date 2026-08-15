# 04 — Group Policy Security

## Overview

This phase documents the configuration of a workstation security baseline using Active Directory Group Policy.

The objective is to centrally configure security controls that will later be applied to Windows 11 domain workstations through a Group Policy Object (GPO) linked to the `Workstations` Organizational Unit (OU).

The GPO is configured on the Domain Controller before the Windows 11 workstation is deployed. This allows the workstation to receive the defined security policies after joining the `adlab.local` domain and being placed in the appropriate OU.

---

## Environment

The primary Domain Controller is:

```text
DC01
```

The relevant Organizational Unit for this phase is:

```text
Workstations
```

The GPO created for workstation security is:

```text
GPO-Workstations-Security
```

The intended relationship is:

```text
adlab.local
      │
      ▼
Workstations OU
      │
      ▼
GPO-Workstations-Security
      │
      ▼
Windows 11 Workstations
```

The purpose of this structure is to allow workstation security policies to be managed centrally.

---

## Why Group Policy Is Used

Group Policy allows administrators to centrally configure security settings for users and computers within an Active Directory environment.

Instead of manually configuring every Windows workstation individually, security settings can be defined within a GPO and linked to an Organizational Unit.

This provides:

* Centralized security configuration
* Consistent security settings
* Easier administration
* Reduced configuration drift
* Easier auditing and maintenance
* Scalable security management

For this laboratory, the GPO is prepared before the Windows 11 workstation is deployed.

The intended workflow is:

```text
Create GPO
    │
    ▼
Configure Security Policies
    │
    ▼
Link GPO to Workstations OU
    │
    ▼
Create Windows 11 Client
    │
    ▼
Join Client to Domain
    │
    ▼
Place Client in Workstations OU
    │
    ▼
Group Policy Applied
    │
    ▼
Validate Configuration
```

This approach allows the Windows 11 client to receive the workstation security baseline after it becomes part of the domain.

---

# GPO Creation

## GPO Name

The following Group Policy Object was created:

```text
GPO-Workstations-Security
```

The GPO was created from Group Policy Management and linked to:

```text
Workstations
```

This means that computers located within the `Workstations` OU are intended to receive the policies configured inside this GPO.

---

## GPO Scope

The GPO is currently linked to the `Workstations` OU within the `adlab.local` domain.

```text
adlab.local
└── Workstations
    └── GPO-Workstations-Security
```

The GPO is intended primarily for workstation computer objects.

The Windows 11 client that will be created in a later phase will be placed within this OU so that the workstation security policies can be evaluated and validated.

---

# Security Policy Configuration

## 1. Audit Logon

The first security control configured was:

```text
Audit Logon
```

Location:

```text
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Advanced Audit Policy Configuration
                └── Audit Policies
                    └── Logon/Logoff
                        └── Audit Logon
```

The following audit events were enabled:

```text
Success
Failure
```

### Purpose

The purpose of this configuration is to provide visibility into successful and failed authentication attempts.

Successful logon events can help identify legitimate access to a workstation.

Failed logon events can help identify:

* Incorrect passwords
* Repeated authentication failures
* Password guessing attempts
* Potential brute-force activity
* Suspicious authentication behavior

The intended security monitoring workflow is:

```text
Authentication Attempt
        │
        ├── Success
        │
        └── Failure
              │
              ▼
       Windows Security Log
              │
              ▼
            Wazuh
              │
              ▼
       Security Monitoring
```

### Evidence

```text
Screenshots/18-gpo-audit-logon-success-failure.png
```

---

# 2. Account Lockout Policy

The second security control configured was the Account Lockout Policy.

Location:

```text
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Account Policies
                └── Account Lockout Policy
```

The following values were configured:

| Policy                              | Configuration            |
| ----------------------------------- | ------------------------ |
| Account lockout threshold           | 5 invalid logon attempts |
| Account lockout duration            | 15 minutes               |
| Reset account lockout counter after | 15 minutes               |

### Security Rationale

The account lockout threshold was configured to:

```text
5 failed attempts
```

The objective is to limit repeated authentication attempts while allowing a reasonable number of legitimate password-entry mistakes.

The configuration provides a balance between:

* Security against password guessing
* Protection against repeated authentication attempts
* Usability for legitimate users
* Avoiding unnecessarily long account lockouts

The intended behavior is:

```text
Failed attempt #1
        │
Failed attempt #2
        │
Failed attempt #3
        │
Failed attempt #4
        │
Failed attempt #5
        │
        ▼
Account Lockout
        │
        ▼
15 minutes
```

The lockout counter is configured to reset after 15 minutes without additional failed authentication attempts.

This means that a legitimate user who makes several mistakes but subsequently stops attempting to authenticate will not remain permanently close to the lockout threshold.

### Security Consideration

Account lockout is a preventive control rather than only a monitoring control.

It can reduce the effectiveness of repeated password-guessing attempts against domain accounts.

The policy will be tested later using a controlled laboratory account after the Windows 11 client and domain authentication environment are operational.

### Evidence

```text
Screenshots/19-gpo-account-lockout-policy.png
```

---

# 3. Audit Policy Change

The third security control configured was:

```text
Audit Audit Policy Change
```

Location:

```text
Computer Configuration
└── Policies
    └── Windows Settings
        └── Security Settings
            └── Advanced Audit Policy Configuration
                └── Audit Policies
                    └── Policy Change
                        └── Audit Audit Policy Change
```

The following audit events were enabled:

```text
Success
Failure
```

## Purpose

Security policies control important aspects of the Windows security environment.

Monitoring changes to audit policies is important because unauthorized modification of security auditing can reduce visibility into malicious activity.

For example, an attacker who obtains elevated privileges could attempt to modify auditing settings to reduce the amount of security telemetry generated by the system.

Monitoring policy changes provides additional visibility into this type of activity.

The intended monitoring workflow is:

```text
Security Policy Change
        │
        ▼
Windows Security Event
        │
        ▼
Wazuh
        │
        ▼
Detection / Investigation
```

### Evidence

```text
Screenshots/20-gpo-audit-policy-change.png
```

---

# 4. PowerShell Script Block Logging

PowerShell is an important Windows administration and automation technology.

It is also frequently involved in security investigations because malicious activity can be performed through legitimate PowerShell functionality.

The following policy was enabled:

```text
Turn on PowerShell Script Block Logging
```

Location:

```text
Computer Configuration
└── Policies
    └── Administrative Templates
        └── Windows Components
            └── Windows PowerShell
                └── Turn on PowerShell Script Block Logging
```

The policy was configured as:

```text
Enabled
```

## Security Rationale

Script Block Logging increases visibility into PowerShell activity by recording executed PowerShell script blocks.

This can provide useful telemetry for:

* Security monitoring
* Threat hunting
* Incident investigation
* Detection engineering
* Identification of suspicious PowerShell activity

The intended workflow is:

```text
PowerShell Execution
        │
        ▼
Script Block Logging
        │
        ▼
Windows Event Log
        │
        ▼
Wazuh
        │
        ▼
Detection / Investigation
```

The actual event generation and Wazuh collection will be validated in a later phase after the Windows 11 endpoint is available.

### Evidence

```text
Screenshots/21-gpo-powershell-script-block-logging.png
```

---

# 5. Microsoft Defender Behavior Monitoring

The next security control configured was Microsoft Defender behavior monitoring.

Location:

```text
Computer Configuration
└── Policies
    └── Administrative Templates
        └── Windows Components
            └── Microsoft Defender Antivirus
                └── Real-time Protection
                    └── Turn on behavior monitoring
```

The policy was configured as:

```text
Enabled
```

## Security Rationale

Behavior monitoring provides an additional endpoint protection mechanism by allowing Microsoft Defender to monitor application and process behavior.

This complements signature-based malware detection by providing behavioral security monitoring.

The objective is to maintain an active endpoint security layer on future Windows workstations.

The intended security model is:

```text
Application / Process
        │
        ▼
Microsoft Defender
        │
        ▼
Behavior Monitoring
        │
        ├── Suspicious Activity
        │
        └── Normal Activity
```

This control represents the preventive security layer of the workstation baseline.

### Evidence

```text
Screenshots/22-gpo-defender-behavior-monitoring.png
```

---

# Defense in Depth

The security controls configured in this phase are designed to provide multiple defensive layers.

```text
                 GPO-Workstations-Security
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
   Authentication      Endpoint         Security
     Security          Protection        Visibility
          │                │                │
          ▼                ▼                ▼
   Account Lockout     Defender        Audit Policies
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                      Windows 11
                           │
                           ▼
                         Wazuh
```

The laboratory therefore separates security controls into several categories.

### Authentication Protection

```text
Account Lockout
```

Provides protection against repeated failed authentication attempts.

### Authentication Visibility

```text
Audit Logon
```

Provides visibility into successful and failed authentication events.

### Security Configuration Visibility

```text
Audit Audit Policy Change
```

Provides visibility into changes to security auditing configuration.

### PowerShell Visibility

```text
PowerShell Script Block Logging
```

Provides telemetry for PowerShell script execution.

### Endpoint Protection

```text
Microsoft Defender Behavior Monitoring
```

Provides behavioral endpoint protection.

---

# Evidence

The following screenshots were captured during this phase:

| Evidence                                     | Description                                            |
| -------------------------------------------- | ------------------------------------------------------ |
| `17-gpo-workstations-linked.png`             | GPO created and linked to the Workstations OU          |
| `18-gpo-audit-logon-success-failure.png`     | Audit Logon configured for Success and Failure         |
| `19-gpo-account-lockout-policy.png`          | Account Lockout configured with 5 / 15 / 15 values     |
| `20-gpo-audit-policy-change.png`             | Audit Policy Change configured for Success and Failure |
| `21-gpo-powershell-script-block-logging.png` | PowerShell Script Block Logging enabled                |
| `22-gpo-defender-behavior-monitoring.png`    | Microsoft Defender Behavior Monitoring enabled         |

All screenshots are stored in:

```text
Screenshots/
```

Sensitive information such as passwords, credentials, tokens, or other secrets is not stored in the repository.

---

# Validation Status

The GPO configuration has been completed.

However, the policies have **not yet been fully validated on a Windows workstation**.

This distinction is important.

The current phase confirms that the policies were configured within the GPO.

The next phase will verify that the policies are actually processed and applied to a Windows 11 domain client.

The planned validation workflow is:

```text
GPO Configuration
        │
        ▼
Windows 11 Client Creation
        │
        ▼
Domain Join
        │
        ▼
Move / Place Computer in Workstations OU
        │
        ▼
Group Policy Processing
        │
        ▼
Policy Verification
        │
        ├── gpresult
        ├── Group Policy Results
        └── Windows Event Viewer
```

The security events generated by these policies will subsequently be used for Wazuh integration and detection engineering.

---

# Current Status

**Current status:** Initial workstation security GPO configured.

## Completed

* [x] Group Policy Management verified on DC01
* [x] `GPO-Workstations-Security` created
* [x] GPO linked to `Workstations` OU
* [x] Audit Logon configured
* [x] Successful logon auditing enabled
* [x] Failed logon auditing enabled
* [x] Account Lockout Policy configured
* [x] Account lockout threshold set to 5 attempts
* [x] Account lockout duration set to 15 minutes
* [x] Account lockout counter reset configured for 15 minutes
* [x] Audit Audit Policy Change configured
* [x] PowerShell Script Block Logging enabled
* [x] Microsoft Defender Behavior Monitoring enabled
* [x] Security configuration documented
* [x] Evidence screenshots captured

## Pending Validation

* [ ] Create Windows 11 client VM
* [ ] Configure Windows 11 networking
* [ ] Configure Windows 11 DNS
* [ ] Join Windows 11 client to `adlab.local`
* [ ] Place `WIN11-01` in `Workstations` OU
* [ ] Verify GPO application
* [ ] Verify audit events
* [ ] Test account lockout behavior
* [ ] Verify PowerShell Script Block Logging
* [ ] Verify Microsoft Defender configuration
* [ ] Generate controlled security events
* [ ] Integrate Windows security telemetry with Wazuh

---

# Next Phase

The next phase of the laboratory will introduce the Windows 11 domain client.

The planned architecture is:

```text
                    Active Directory
                      adlab.local
                           │
                           ▼
                         DC01
                  Windows Server 2022
                           │
                           │
                    Workstations OU
                           │
                           ▼
                       WIN11-01
                    Windows 11 Client
                           │
                           ▼
                GPO-Workstations-Security
                           │
                           ▼
                    Security Controls
                           │
                           ▼
                         Wazuh
                           │
                           ▼
                  Detection / Analysis
```

The Windows 11 workstation will provide the endpoint required to validate the policies configured in this phase and generate controlled security telemetry for subsequent monitoring and detection engineering.

---

# Security Design Principles

The configuration performed in this phase follows several security principles.

## Least Privilege

Security policies are applied according to the role and purpose of the system rather than granting unnecessary privileges.

## Centralized Management

Security controls are managed centrally through Active Directory Group Policy instead of manually configuring individual workstations.

## Defense in Depth

Multiple security layers are implemented instead of relying on a single security mechanism.

## Visibility

Authentication, policy changes, and PowerShell activity are logged to provide useful security telemetry.

## Prevention

Account Lockout and Microsoft Defender provide preventive security controls in addition to monitoring.

## Controlled Validation

Security controls will be tested using controlled laboratory scenarios rather than production systems.

---

# Disclaimer

This project is intended for educational and defensive cybersecurity purposes.

All security testing and experimentation is performed within a controlled laboratory environment owned or administered by the author.

No production systems or third-party environments are intentionally targeted as part of this project.