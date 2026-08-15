## Windows Server 2022 Cybersecurity Lab

## Overview

This project documents the deployment, configuration, and security implementation of a Windows Server 2022 virtual machine as part of a personal cybersecurity home lab.

The objective is to build a controlled Windows Server environment that can be used to practice system administration, networking, identity and access management, Active Directory, security monitoring, detection engineering, and defensive security techniques.

The laboratory is designed to progressively evolve from a basic Windows Server deployment into a small enterprise-style security environment integrated with a Windows endpoint and Wazuh SIEM/XDR.

The entire environment is documented to provide a reproducible laboratory and demonstrate practical cybersecurity skills.

---

## Objectives

The main objectives of this laboratory are:

* Deploy Windows Server 2022 in a virtualized environment.
* Configure and administer a Windows Server system.
* Establish a secure baseline configuration.
* Configure networking and DNS services.
* Implement Active Directory Domain Services.
* Configure identity and access management concepts.
* Apply the principle of least privilege.
* Implement security groups and delegated administration.
* Configure Group Policy Objects.
* Establish a workstation security baseline.
* Join a Windows 11 endpoint to the domain.
* Generate and investigate Windows security events.
* Integrate Active Directory security events with Wazuh.
* Develop and test detection rules.
* Practice defensive security and system hardening.
* Document the complete process in a reproducible format.

---

## Lab Environment

| Component          | Configuration                      |
| ------------------ | ---------------------------------- |
| Hypervisor         | VMware Workstation Pro             |
| Operating System   | Windows Server 2022 Standard       |
| Installation       | Desktop Experience                 |
| Architecture       | x64                                |
| CPU                | 4 vCPU                             |
| RAM                | 4 GB                               |
| Storage            | 80 GB                              |
| Network            | NAT                                |
| Installation Media | Windows Server 2022 Evaluation ISO |
| Server Hostname    | DC01                               |
| Active Directory   | adlab.local                        |
| NetBIOS Domain     | ADLAB                              |
| Server IPv4        | 192.168.100.128                    |

---

## Project Structure

```text
Windows-Server-2022-Lab/
│
├── README.md
│
├── Docs/
│   ├── 01-vm-creation.md
│   ├── 02-windows-server-installation.md
│   ├── 03-active-directory-domain-services.md
│   └── 04-group-policy-security.md
│
└── Screenshots/
    ├── 01-first-server-desktop.png
    ├── 02-hostname-configured.png
    ├── 03-static-ip-configuration.png
    ├── 04-ad-ds-role-selected.png
    ├── 05-ad-ds-installation-complete.png
    ├── 06-new-forest-adlab.png
    ├── 07-domain-controller-options.png
    ├── 08-netbios-domain-name.png
    ├── 09-ad-ds-review-options.png
    ├── 10-domain-controller-login.png
    ├── 11-active-directory-domain.png
    ├── 12-active-directory-ous.png
    ├── 13-ad-user-created.png
    ├── 14-user-group-membership.png
    ├── 15-soc-analysts-group.png
    ├── 16-ad-delegation-users-lab.png
    ├── 17-gpo-workstations-linked.png
    ├── 18-gpo-audit-logon-success-failure.png
    ├── 19-gpo-account-lockout-policy.png
    ├── 20-gpo-audit-policy-change.png
    ├── 21-gpo-powershell-script-block-logging.png
    └── 22-gpo-defender-behavior-monitoring.png
```

Additional documentation and evidence will be added as the laboratory progresses.

---

## Documentation

### Phase 1 — Virtual Machine Creation

Documents the creation and configuration of the VMware virtual machine, including CPU, memory, storage, networking, installation media, and initial boot troubleshooting.

→ [`01-vm-creation.md`](Docs/01-vm-creation.md)

### Phase 2 — Windows Server Installation

Documents the installation of Windows Server 2022 Standard with Desktop Experience, including disk selection, installation type, administrator configuration, and first successful boot.

→ [`02-windows-server-installation.md`](Docs/02-windows-server-installation.md)

### Phase 3 — Active Directory Domain Services

Documents the configuration of the Windows Server environment as an Active Directory Domain Controller.

This phase includes:

* Server hostname configuration
* Static IP configuration
* DNS configuration
* Active Directory Domain Services installation
* Domain Controller promotion
* Active Directory forest creation
* Domain configuration
* NetBIOS configuration
* DNS verification
* Organizational Unit creation
* User account creation
* Security group creation
* SOC-Analysts group configuration
* Privileged account separation
* AD-Admins security group
* Delegated administration
* Least privilege implementation
* Delegation validation

→ [`03-active-directory-domain-services.md`](Docs/03-active-directory-domain-services.md)

### Phase 4 — Group Policy Security

Documents the creation and configuration of a workstation security baseline using Active Directory Group Policy.

This phase includes:

* Group Policy Management configuration
* `GPO-Workstations-Security` creation
* GPO linkage to the `Workstations` OU
* Audit Logon configuration
* Successful logon auditing
* Failed logon auditing
* Account Lockout Policy
* Audit Audit Policy Change
* PowerShell Script Block Logging
* Microsoft Defender Behavior Monitoring
* Centralized workstation security configuration
* Defense-in-depth security controls
* Security evidence collection

→ [`04-group-policy-security.md`](Docs/04-group-policy-security.md)

---

## Active Directory Environment

The current laboratory domain is:

```text
adlab.local
```

The primary Domain Controller is:

```text
DC01
```

The NetBIOS domain name is:

```text
ADLAB
```

The current Organizational Unit structure is:

```text
adlab.local
│
├── Admins
├── Groups
├── Users-Lab
├── Servers
└── Workstations
```

Security groups currently include:

```text
SOC-Analysts
AD-Admins
```

The laboratory also contains separate administrative and standard user identities in order to demonstrate privilege separation and delegated administration.

---

## Identity and Access Management

The laboratory demonstrates several Active Directory identity and access management concepts.

### Standard Users

Standard user accounts are maintained inside the `Users-Lab` OU.

These accounts are intended to represent normal domain users without administrative privileges.

### SOC-Analysts

The `SOC-Analysts` security group represents users who require permissions or access related to SOC activities.

### AD-Admins

The `AD-Admins` security group represents administrators who require delegated Active Directory management capabilities.

The administrative account `maria.admin` was assigned to this group.

### Delegated Administration

Administrative permissions were delegated to `AD-Admins` over the `Users-Lab` OU.

The delegated permissions include:

* Create, delete, and manage user accounts
* Reset user passwords and force password change at next logon

This demonstrates the principle of least privilege by providing specific administrative capabilities without assigning unrestricted Domain Admin privileges.

The delegation was validated through a controlled password-reset operation performed by `maria.admin` against a user in `Users-Lab`.

---

## Group Policy Security

The workstation security baseline is managed centrally through:

```text
GPO-Workstations-Security
```

The GPO is associated with the:

```text
Workstations
```

OU.

The purpose of this configuration is to ensure that Windows workstations receive a consistent security baseline after joining the domain.

The following security controls have been configured:

* Audit Logon
* Successful logon auditing
* Failed logon auditing
* Account Lockout Policy
* Audit Audit Policy Change
* PowerShell Script Block Logging
* Microsoft Defender Behavior Monitoring

The GPO is currently configured on the Domain Controller.

Full policy validation will occur after the Windows 11 client is created, joined to the domain, and placed in the `Workstations` OU.

→ [`04-group-policy-security.md`](Docs/04-group-policy-security.md)

---

## Virtualization

The Windows Server environment is hosted in VMware Workstation Pro.

The virtual machine currently uses a NAT network configuration, providing Internet connectivity through the host system while avoiding direct exposure of the server to the physical network.

The laboratory environment is intended to remain controlled and isolated from production systems.

---

## Security Focus

The laboratory will progressively incorporate defensive security practices including:

* Active Directory security
* Identity and access management
* Least privilege
* Privileged account separation
* Group Policy security
* Windows security configuration
* Firewall configuration
* Event logging
* Security auditing
* PowerShell logging
* System monitoring
* Authentication monitoring
* Account lockout protection
* Microsoft Defender endpoint protection
* Active Directory security event analysis
* Detection engineering
* Security event investigation
* Hardening techniques
* Wazuh SIEM/XDR integration

The environment is designed to generate realistic but controlled security events that can later be investigated through Wazuh.

---

## Planned Security Detection Scenarios

Once the Active Directory environment and Windows 11 endpoint are fully configured, controlled security scenarios will be generated for detection and analysis.

Examples include:

* Multiple failed authentication attempts
* Successful and failed logons
* Account lockout events
* User creation
* User deletion
* Password resets
* Group membership changes
* Privilege changes
* Administrative activity
* PowerShell execution
* Group Policy modifications
* Other Active Directory administrative activity

These events will provide realistic telemetry for security monitoring and detection engineering.

---

## Planned Architecture

The final laboratory architecture is planned as:

```text
                    Active Directory
                      adlab.local
                           │
                           ▼
                         DC01
                  Windows Server 2022
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

The architecture will be expanded as additional laboratory components are implemented.

---

## Evidence

Screenshots documenting significant configuration and testing steps are stored in the `Screenshots/` directory.

Screenshots are used as supporting evidence for the configuration and implementation described in the documentation.

Evidence currently includes:

* Windows Server installation
* Hostname configuration
* Static IP configuration
* AD DS installation
* Domain Controller promotion
* Active Directory domain configuration
* Organizational Unit structure
* User creation
* Group membership
* SOC-Analysts security group
* Delegated administration configuration
* GPO creation and linkage
* Audit Logon configuration
* Account Lockout configuration
* Audit Policy Change configuration
* PowerShell Script Block Logging configuration
* Microsoft Defender Behavior Monitoring configuration

The delegated administration functionality was successfully validated through a controlled password-reset operation. No separate screenshot was captured for this validation.

The Group Policy configuration has been documented with screenshots. Full workstation-side validation will be performed after the Windows 11 client is deployed.

Sensitive information such as passwords, credentials, tokens, or other secrets will not be stored in the repository.

---

## Status

**Current status:** Active Directory Domain Services and initial workstation security baseline successfully deployed and configured.

### Completed

* [x] VMware virtual machine created
* [x] Windows Server 2022 Evaluation ISO configured
* [x] VM boot troubleshooting completed
* [x] Windows Server 2022 Standard installed
* [x] Desktop Experience installed
* [x] Administrator account configured
* [x] First successful boot completed
* [x] Server hostname configured
* [x] Static IP configured
* [x] DNS configured
* [x] Active Directory Domain Services installed
* [x] Domain Controller promotion completed
* [x] Active Directory forest created
* [x] `adlab.local` domain configured
* [x] NetBIOS domain name configured
* [x] DNS resolution verified
* [x] Organizational Units created
* [x] Standard user account created
* [x] SOC-Analysts security group created
* [x] Privileged administrative account created
* [x] AD-Admins security group created
* [x] Delegated administration configured
* [x] Delegated administration successfully validated
* [x] Group Policy Management configured
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
* [x] Phase 4 documentation created

### In Progress

* [ ] Create Windows 11 client virtual machine
* [ ] Configure Windows 11 networking
* [ ] Configure Windows 11 DNS
* [ ] Join Windows 11 client to the domain
* [ ] Place `WIN11-01` in the `Workstations` OU
* [ ] Verify GPO application
* [ ] Configure domain authentication
* [ ] Verify audit events
* [ ] Test account lockout behavior
* [ ] Verify PowerShell Script Block Logging
* [ ] Verify Microsoft Defender configuration
* [ ] Configure security auditing
* [ ] Generate controlled security events
* [ ] Integrate Windows security telemetry with Wazuh
* [ ] Develop detection and monitoring use cases
* [ ] Implement additional security controls

---

## Future Lab Development

The laboratory will progressively evolve into a multi-system cybersecurity environment.

Future stages will include:

1. Windows 11 domain client deployment
2. Domain authentication testing
3. Group Policy validation
4. Security auditing validation
5. Controlled security scenarios
6. Wazuh integration
7. Detection engineering
8. Threat hunting
9. Security event investigation
10. Documentation of detection use cases

The final objective is to demonstrate an end-to-end defensive security workflow:

```text
Configuration
      │
      ▼
Security Event
      │
      ▼
Log Collection
      │
      ▼
Wazuh
      │
      ▼
Detection
      │
      ▼
Investigation
      │
      ▼
Analysis
```

---

## Disclaimer

This project is intended for educational and defensive cybersecurity purposes.

All security testing and experimentation is performed within a controlled laboratory environment owned or administered by the author.

No production systems or third-party environments are intentionally targeted as part of this project.