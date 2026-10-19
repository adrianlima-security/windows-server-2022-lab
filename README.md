Windows Server 2022 Cybersecurity Lab

Overview

This project documents the deployment, configuration, and security implementation of a Windows Server 2022 virtual machine as part of a personal cybersecurity home lab.

The objective is to build a controlled Windows Server environment to practice system administration, networking, identity and access management, Active Directory, security monitoring, detection engineering, and defensive security techniques.

The laboratory progressively evolves into an enterprise-style security environment with a Windows 11 domain client and Wazuh SIEM/XDR integration.

All phases are documented with technical explanations and screenshots to demonstrate practical cybersecurity skills and support reproducibility.

Objectives

Deploy Windows Server 2022 in a virtualized environment.

Configure Windows Server networking and DNS.

Implement Active Directory Domain Services.

Configure users, security groups, and organizational units.

Apply the principle of least privilege.

Implement delegated administration.

Configure Group Policy Objects.

Establish a workstation security baseline.

Join a Windows 11 endpoint to the domain.

Validate Group Policy application on the client.

Enable and verify PowerShell Script Block Logging.

Investigate Windows security events.

Deploy Wazuh SIEM/XDR on Ubuntu Server.

Integrate Windows endpoint and Active Directory telemetry with Wazuh.

Develop and test defensive detection scenarios.

Document implementation steps and supporting evidence.

Lab Environment

Component

Configuration

Hypervisor

VMware Workstation Pro

Server OS

Windows Server 2022 Standard

Installation

Desktop Experience

Architecture

x64

Server CPU

4 vCPU

Server RAM

4 GB

Server Storage

80 GB

Network

NAT

Server Hostname

DC01

Active Directory Domain

adlab.local

NetBIOS Domain

ADLAB

Domain Controller IP

192.168.100.10

Client OS

Windows 11 Pro

VMware Client VM Name

WIN11-CLIENT-2

Active Directory Computer Name

DESKTOP-HGSPMK5

Client DNS Server

192.168.100.10

Security GPO

GPO-Workstations-Security

Client Organizational Unit

Workstations

SIEM/XDR Platform

Wazuh

SIEM Server OS

Ubuntu Server 24.04 LTS

SIEM Deployment

Wazuh All-in-One

SIEM Components

Wazuh Indexer, Manager, Dashboard

SIEM Virtual Disk

60 GB

The Wazuh server is deployed separately from the Windows Server domain controller. Centralized event collection from the Windows systems remains to be configured and validated.

Project Structure

Windows-Server-2022-Lab/
│
├── README.md
│
├── Docs/
│   ├── 01-vm-creation.md
│   ├── 02-windows-server-installation.md
│   ├── 03-active-directory-domain-services.md
│   ├── 04-group-policy-security.md
│   └── 05-windows11-domain-join.md
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
    ├── 22-gpo-defender-behavior-monitoring.png
    ├── 23-win11-client-vm-configuration.png
    ├── 24-windows11-client-installation.png
    ├── 25-win11-dc01-ping.png
    ├── 26-win11-domain-dns-resolution.png
    ├── 27-win11-domain-join-success.png
    ├── 28-win11-applied-group-policy.png
    ├── 29-win11-powershell-logging.png
    └── 30-win11-powershell-event-4104.png

Documentation

Phase 1 — Virtual Machine Creation

Documents the creation and configuration of the Windows Server virtual machine in VMware Workstation Pro, including CPU, memory, storage, networking, installation media, and initial boot troubleshooting.

Documentation: Docs/01-vm-creation.md

Phase 2 — Windows Server Installation

Documents the installation of Windows Server 2022 Standard with Desktop Experience, including disk selection, installation configuration, administrator setup, and first successful boot.

Documentation: Docs/02-windows-server-installation.md

Phase 3 — Active Directory Domain Services

Documents the configuration of the Windows Server environment as an Active Directory Domain Controller.

This phase includes:

Server hostname configuration.

Static IP and DNS configuration.

Active Directory Domain Services installation.

Domain Controller promotion.

Active Directory forest creation.

Domain and NetBIOS configuration.

DNS verification.

Organizational Unit creation.

User account creation.

Security group creation.

SOC-Analysts group configuration.

Privileged account separation.

AD-Admins security group.

Delegated administration.

Least privilege implementation.

Delegation validation.

Documentation: Docs/03-active-directory-domain-services.md

Phase 4 — Group Policy Security

Documents the creation and configuration of a centralized workstation security baseline using Active Directory Group Policy.

This phase includes:

Group Policy Management configuration.

GPO-Workstations-Security creation.

GPO linkage to the Workstations OU.

Successful and failed logon auditing.

Account Lockout Policy.

Audit Policy Change.

PowerShell Script Block Logging.

Microsoft Defender behavior monitoring.

Centralized workstation security configuration.

Security evidence collection.

Documentation: Docs/04-group-policy-security.md

Phase 5 — Windows 11 Domain Join and Security Validation

Documents the deployment of a Windows 11 Pro client and its integration into the Active Directory environment.

This phase includes:

Windows 11 virtual machine deployment.

Windows 11 Pro installation.

Network connectivity verification.

DNS configuration pointing to DC01.

Domain Controller connectivity testing.

DNS resolution for adlab.local.

Domain join verification.

Computer account placement in the Workstations OU.

Application of GPO-Workstations-Security.

Verification of PowerShell Script Block Logging.

Inspection of the PowerShell Operational event log.

Validation of Event ID 4104.

Screenshot collection and technical documentation.

The client successfully joined the adlab.local domain. Its computer account, DESKTOP-HGSPMK5, was moved to the Workstations OU. Following a Group Policy refresh, GPO-Workstations-Security appeared in the client's computer policy results.

The registry value EnableScriptBlockLogging was confirmed as 1, and events with ID 4104 were observed in the Microsoft-Windows-PowerShell/Operational log.

These validations demonstrate that the relevant Group Policy settings were applied locally and that PowerShell script block logging events were generated.

Documentation: Docs/05-windows11-domain-join.md

Active Directory Environment

Domain: adlab.local

Domain Controller: DC01

Server IP: 192.168.100.10

NetBIOS Domain: ADLAB

The current Organizational Unit structure is:

adlab.local
│
├── Admins
├── Groups
├── Users-Lab
├── Servers
└── Workstations

Security groups include:

SOC-Analysts

AD-Admins

The laboratory uses separate administrative and standard user identities to demonstrate privilege separation and delegated administration.

Identity and Access Management

Standard Users

Standard user accounts are maintained in the Users-Lab OU and represent normal domain users without administrative privileges.

SOC-Analysts

The SOC-Analysts security group represents users who require access related to Security Operations Center activities.

AD-Admins

The AD-Admins security group represents administrators with delegated Active Directory management capabilities.

The administrative account maria.admin was assigned to this group.

Delegated Administration

Administrative permissions were delegated to AD-Admins over the Users-Lab OU.

Delegated capabilities include:

Creating, deleting, and managing user accounts.

Resetting user passwords.

Requiring a password change at the next logon.

The delegation was validated through a controlled password-reset operation performed by maria.admin against a user in Users-Lab.

This demonstrates the principle of least privilege by granting specific administrative capabilities without assigning unrestricted Domain Admin privileges.

Group Policy Security

The workstation security baseline is managed through:

GPO-Workstations-Security

The GPO is linked to the Workstations OU.

The configured security controls include:

Audit Logon.

Successful logon auditing.

Failed logon auditing.

Account Lockout Policy.

Audit Policy Change.

PowerShell Script Block Logging.

Microsoft Defender behavior monitoring.

The Windows 11 client was joined to adlab.local and placed in the Workstations OU. After a Group Policy refresh, the client reported GPO-Workstations-Security in its computer policy results.

PowerShell Script Block Logging was validated by checking the registry configuration and inspecting the PowerShell Operational event log.

These results demonstrate workstation-side Group Policy application and the generation of PowerShell script block logging events.

Windows 11 Client Validation

The Windows 11 Pro endpoint was deployed in VMware Workstation Pro.

The following validations were completed:

Successful installation of Windows 11 Pro.

Network connectivity to DC01 at 192.168.100.10.

DNS resolution of adlab.local.

Successful domain join.

Placement of the client computer account in the Workstations OU.

Application of GPO-Workstations-Security.

Confirmation that EnableScriptBlockLogging is set to 1.

Observation of Event ID 4104 in Microsoft-Windows-PowerShell/Operational.

The VMware virtual machine is named WIN11-CLIENT-2. The computer account registered in Active Directory is DESKTOP-HGSPMK5.

The Windows client has not yet been integrated with Wazuh. Centralized event collection and detection validation remain pending.

Wazuh SIEM/XDR Integration

Wazuh has been installed on an Ubuntu Server 24.04 LTS virtual machine as an All-in-One deployment.

The deployment includes:

Wazuh Indexer.

Wazuh Manager.

Wazuh Dashboard.

The Wazuh dashboard is accessible, confirming that the platform has been deployed and its web interface is available.

The Windows Server 2022 domain controller and Windows 11 domain client have not yet been integrated with the Wazuh manager.

Planned Integration Tasks

Verify the status of Wazuh services.

Confirm network connectivity between the Wazuh server and Windows systems.

Install and configure the Wazuh agent on Windows 11.

Install and configure the Wazuh agent on DC01.

Verify agent registration and connectivity.

Confirm centralized collection of Windows security events.

Validate PowerShell event collection.

Generate controlled authentication and account-management events.

Investigate alerts in the Wazuh dashboard.

Develop and document detection use cases.

The integration will connect the existing Windows security configuration to centralized monitoring and detection.

Virtualization and Networking

The Windows Server environment is hosted in VMware Workstation Pro using a NAT network configuration.

The Windows 11 client uses DC01 as its preferred DNS server to resolve the Active Directory domain.

The Wazuh server runs on a separate Ubuntu Server virtual machine. Network communication between the SIEM and the Windows systems must be verified during the integration phase.

The lab is intended for controlled testing and defensive security exercises, separate from production systems.

Security Focus

The laboratory progressively incorporates defensive security practices, including:

Active Directory security.

Identity and access management.

Least privilege.

Privileged account separation.

Group Policy security.

Windows endpoint hardening.

Firewall configuration.

Event logging and auditing.

PowerShell script logging.

Authentication monitoring.

Account lockout protection.

Microsoft Defender endpoint protection.

Active Directory security event analysis.

Centralized log collection.

SIEM monitoring.

Detection engineering.

Security event investigation.

Threat hunting.

Planned Security Detection Scenarios

Once centralized monitoring is implemented, the lab will be used to generate controlled security events for investigation and detection development.

Planned scenarios include:

Multiple failed authentication attempts.

Successful and failed logons.

Account lockout events.

User creation and deletion.

Password resets.

Group membership changes.

Privilege changes.

Administrative activity.

PowerShell execution.

Group Policy modifications.

Other Active Directory administrative activity.

These scenarios will provide telemetry for security monitoring and detection engineering.

Planned Architecture

             Active Directory Domain
                  adlab.local
                      |
                      v
                    DC01
             Windows Server 2022
                      |
                      v
              Workstations OU
                      |
                      v
             Windows 11 Client
              WIN11-CLIENT-2
                      |
                      v
          GPO-Workstations-Security
                      |
                      v
             Windows Event Logs
                      |
                      v
              Wazuh Agents
                      |
                      v
              Wazuh Manager
                      |
                      v
             Detection and Analysis

The diagram represents the intended monitoring architecture. Agent deployment, centralized event collection, and detection validation are still pending.

Evidence

Screenshots documenting significant configuration and validation steps are stored in the Screenshots/ directory.

Evidence includes:

Windows Server installation.

Hostname and static IP configuration.

Active Directory Domain Services installation.

Domain Controller promotion.

Active Directory domain configuration.

Organizational Unit structure.

User and security group creation.

Delegated administration configuration.

Group Policy creation and linkage.

Audit Logon configuration.

Account Lockout configuration.

Audit Policy Change configuration.

PowerShell Script Block Logging configuration.

Microsoft Defender behavior monitoring configuration.

Windows 11 virtual machine configuration and installation.

Connectivity between Windows 11 and DC01.

DNS resolution for the domain.

Windows 11 domain join.

Workstation GPO application.

PowerShell Script Block Logging registry configuration.

PowerShell Operational Event ID 4104.

Additional screenshots will be collected during Wazuh agent deployment, event collection validation, and detection testing.

The screenshots provide supporting evidence for the steps described in the phase documentation.

Sensitive information such as passwords, credentials, tokens, private keys, and other secrets must not be committed to the repository.

Status

Current status: Active Directory and the Windows 11 domain client are configured. Workstation Group Policy application and PowerShell Script Block Logging have been validated locally. Wazuh is installed on Ubuntu Server, and its dashboard is accessible. Integration between Wazuh and the Windows systems remains pending.

Completed

VMware virtual machine created.

Windows Server 2022 Standard installed.

Desktop Experience installed.

Administrator account configured.

Server hostname configured.

Static IP and DNS configured.

Active Directory Domain Services installed.

Domain Controller promotion completed.

adlab.local forest and domain configured.

NetBIOS domain name configured.

DNS resolution verified.

Organizational Units created.

Standard user account created.

SOC-Analysts security group created.

Privileged administrative account created.

AD-Admins security group created.

Delegated administration configured and validated.

GPO-Workstations-Security created.

GPO linked to the Workstations OU.

Audit Logon configured.

Successful and failed logon auditing enabled.

Account Lockout Policy configured.

Account lockout threshold set to 5 attempts.

Account lockout duration set to 15 minutes.

Account lockout counter reset configured for 15 minutes.

Audit Policy Change configured.

PowerShell Script Block Logging configured.

Microsoft Defender behavior monitoring configured.

Windows 11 Pro client deployed.

Windows 11 connectivity to DC01 verified.

DNS resolution for adlab.local verified.

Windows 11 client joined to the domain.

Client computer account moved to the Workstations OU.

Workstation GPO application verified.

PowerShell Script Block Logging registry value verified.

PowerShell Operational Event ID 4104 observed.

Phase 5 documentation and screenshots collected.

Wazuh All-in-One deployed on Ubuntu Server.

Wazuh dashboard accessed successfully.

In Progress / Planned

Verify domain authentication scenarios.

Test account lockout behavior.

Verify Microsoft Defender configuration on the client.

Generate additional controlled security events.

Configure the Wazuh agent on Windows 11.

Configure the Wazuh agent on DC01.

Verify agent connectivity and status.

Verify centralized Windows event collection.

Validate PowerShell event collection in Wazuh.

Develop detection and monitoring use cases.

Investigate collected security events.

Implement additional security controls.

Document Wazuh alerts and detection results.

Future Lab Development

Future stages will include:

Domain authentication testing.

Additional Group Policy validation.

Security auditing validation.

Controlled security scenarios.

Wazuh agent integration.

Centralized log collection.

Detection engineering.

Threat hunting.

Security event investigation.

Documentation of detection use cases.

The final objective is to demonstrate an end-to-end defensive security workflow:

Configuration
     |
     v
Security Event
     |
     v
Log Collection
     |
     v
Wazuh SIEM
     |
     v
Detection
     |
     v
Investigation
     |
     v
Analysis

Remote Administration with PowerShell Remoting

PowerShell Remoting was previously configured and validated for remote access to the Windows Server 2022 domain controller.

Environment

Domain Controller: DC01

Domain: adlab.local

Server IP: 192.168.100.10

Remote management: PowerShell Remoting (WinRM)

Connection Example

$cred = Get-Credential
Enter-PSSession -ComputerName 192.168.100.10 -Credential $cred

Validation Example

hostname
Get-Service DNS, NTDS, WinRM

The remote session was previously used to confirm the server hostname and inspect the status of DNS, Active Directory Domain Services (NTDS), and WinRM.

Remote administration should be restricted to authorized accounts and trusted hosts. Production environments require appropriate authentication, access restrictions, and transport security.

Disclaimer

This project is intended for educational and defensive cybersecurity purposes.

All security testing and experimentation is performed within a controlled laboratory environment owned or administered by the author.

No production systems or third-party environments are intentionally targeted as part of this project.