
# Windows Server 2022 Cybersecurity Lab

## Overview

This project documents the deployment, configuration, and security implementation of a Windows Server 2022 environment as part of a personal cybersecurity home lab.

The objective is to build a controlled Windows Server environment to practice system administration, networking, identity and access management, Active Directory, security monitoring, detection engineering, and defensive security techniques.

The laboratory progressively evolves into an enterprise-style security environment integrating:

- A Windows Server 2022 Domain Controller.
- A Windows 11 domain client.
- A centralized Wazuh SIEM/XDR platform running on Ubuntu Server.

The project includes Active Directory administration, Group Policy security controls, Windows endpoint monitoring, centralized event collection, custom detection rules, and security incident investigation.

All implementation phases are documented with technical explanations, configuration details, and screenshots to demonstrate practical cybersecurity skills and support reproducibility.

## Objectives

- Deploy Windows Server 2022 in a virtualized environment.
- Configure Windows Server networking and DNS.
- Implement Active Directory Domain Services.
- Configure users, security groups, and Organizational Units.
- Apply the principle of least privilege.
- Implement delegated administration.
- Configure Group Policy Objects.
- Establish a workstation security baseline.
- Join a Windows 11 endpoint to the Active Directory domain.
- Validate Group Policy application on the client.
- Enable and verify PowerShell Script Block Logging.
- Investigate Windows security events.
- Deploy Wazuh SIEM/XDR on Ubuntu Server.
- Install and configure Wazuh agents on Windows 11 and Windows Server 2022.
- Validate agent registration and connectivity.
- Monitor Windows endpoint telemetry.
- Collect and analyze Windows Security events.
- Develop and test custom detection rules.
- Detect repeated failed authentication attempts.
- Correlate security events using Wazuh frequency-based rules.
- Map detection scenarios to MITRE ATT&CK.
- Investigate security alerts through the Wazuh Dashboard.
- Document implementation steps and supporting evidence.

## Lab Environment

| Component | Configuration |
|---|---|
| Hypervisor | VMware Workstation Pro |
| Server OS | Windows Server 2022 Standard |
| Installation | Desktop Experience |
| Architecture | x64 |
| Server CPU | 4 vCPU |
| Server RAM | 4 GB |
| Server Storage | 80 GB |
| Network | NAT |
| Server Hostname | DC01 |
| Active Directory Domain | adlab.local |
| NetBIOS Domain | ADLAB |
| Domain Controller IP | 192.168.100.10 |
| Client OS | Windows 11 Pro |
| VMware Client VM Name | WIN11-CLIENT-2 |
| Active Directory Computer Name | DESKTOP-HGSPMK5 |
| Client DNS Server | 192.168.100.10 |
| Security GPO | GPO-Workstations-Security |
| Client Organizational Unit | Workstations |
| SIEM/XDR Platform | Wazuh |
| SIEM Server OS | Ubuntu Server 24.04 LTS |
| SIEM Server IP | 192.168.100.132 |
| SIEM Deployment | Wazuh All-in-One |
| SIEM Components | Wazuh Indexer, Manager, Dashboard |
| SIEM Virtual Disk | 60 GB |

The Wazuh server is deployed separately from the Windows Server Domain Controller. Both Windows systems are integrated as monitored endpoints through their respective Wazuh agents.

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
│   ├── 04-group-policy-security.md
│   ├── 05-windows11-domain-join.md
│   ├── 06-wazuh-agent-windows11.md
│   ├── 07-wazuh-agent-windows-server.md
│   └── 08-wazuh-windows-security-detection.md
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
    ├── 30-win11-powershell-event-4104.png
    ├── 37-wazuh-agent-install-command.png
    ├── 38-wazuh-agent-service-running.png
    ├── 39-wazuh-agent-active-dashboard.png
    ├── 40-wazuh-windows11-event-detection.png
    ├── 41-dc01-connectivity-to-wazuh.png
    ├── 42-wazuh-manager-port-1514.png
    ├── 43-dc01-tcp-connectivity-1514.png
    ├── 44-wazuh-manager-service-status.png
    ├── 45-wazuh-agent-installation-wizard.png
    ├── 46-dc01-agent-active-wazuh-manager.png
    ├── 47-dc01-agent-active-dashboard.png
    ├── 56-dc01-failed-logon-event-4625-wazuh.png
    ├── 57-dc01-failed-logon-user-count.png
    ├── 58-dc01-failed-logon-investigation-details.png
    ├── 59-wazuh-rule-60122-firedtimes-analysis.png
    ├── 60-wazuh-custom-brute-force-detection-rule-100100.png
    ├── 61-wazuh-custom-brute-force-rule-corrected.png
    ├── 62-wazuh-dashboard-brute-force-alert.png
    ├── 63-wazuh-brute-force-alert-investigation-details.png
    ├── 64-dc01-failed-logons-timeline.png
    └── 65-wazuh-brute-force-correlation-confirmation.png
```

The screenshot sequence preserves the existing numbering. Screenshots 48–55 are part of the existing evidence sequence and are not renamed here.

## Documentation

### Phase 1 — Virtual Machine Creation

Documents the creation and configuration of the Windows Server virtual machine in VMware Workstation Pro.

This phase includes:

- Virtual machine creation.
- CPU and memory allocation.
- Virtual disk configuration.
- NAT network configuration.
- Windows Server installation media.
- Initial boot and installation preparation.

Documentation: `Docs/01-vm-creation.md`

### Phase 2 — Windows Server Installation

Documents the installation of Windows Server 2022 Standard with Desktop Experience.

This phase includes:

- Windows Server installation.
- Disk selection.
- Installation configuration.
- Administrator account setup.
- Initial system boot.
- First login and desktop verification.

Documentation: `Docs/02-windows-server-installation.md`

### Phase 3 — Active Directory Domain Services

Documents the configuration of Windows Server 2022 as an Active Directory Domain Controller.

This phase includes:

- Server hostname configuration.
- Static IP and DNS configuration.
- Active Directory Domain Services installation.
- Domain Controller promotion.
- Active Directory forest creation.
- Domain and NetBIOS configuration.
- DNS verification.
- Organizational Unit creation.
- User account creation.
- Security group creation.
- SOC-Analysts group configuration.
- Privileged account separation.
- AD-Admins security group.
- Delegated administration.
- Least privilege implementation.
- Delegation validation.

The Active Directory domain was configured as `adlab.local`, with `ADLAB` as its NetBIOS name.

Documentation: `Docs/03-active-directory-domain-services.md`

### Phase 4 — Group Policy Security

Documents the creation and configuration of a centralized workstation security baseline using Active Directory Group Policy.

This phase includes:

- Group Policy Management configuration.
- GPO-Workstations-Security creation.
- GPO linkage to the Workstations OU.
- Successful and failed logon auditing.
- Account Lockout Policy.
- Audit Policy Change.
- PowerShell Script Block Logging.
- Microsoft Defender behavior monitoring.
- Centralized workstation security configuration.
- Security evidence collection.

Documentation: `Docs/04-group-policy-security.md`

### Phase 5 — Windows 11 Domain Join and Security Validation

Documents the deployment of a Windows 11 Pro client and its integration into the Active Directory environment.

This phase includes:

- Windows 11 virtual machine deployment.
- Windows 11 Pro installation.
- Network connectivity verification.
- DNS configuration pointing to DC01.
- Domain Controller connectivity testing.
- DNS resolution for `adlab.local`.
- Domain join verification.
- Computer account placement in the Workstations OU.
- Application of GPO-Workstations-Security.
- Verification of PowerShell Script Block Logging.
- Inspection of the PowerShell Operational event log.
- Validation of Event ID 4104.
- Screenshot collection and technical documentation.

The Windows 11 client successfully joined the `adlab.local` domain. Its computer account, `DESKTOP-HGSPMK5`, was moved to the Workstations OU.

Following a Group Policy refresh, `GPO-Workstations-Security` appeared in the client's computer policy results.

The registry value `EnableScriptBlockLogging` was confirmed as `1`, and events with ID 4104 were observed in the `Microsoft-Windows-PowerShell/Operational` log.

Documentation: `Docs/05-windows11-domain-join.md`

### Phase 6 — Wazuh Agent Integration: Windows 11

Documents the deployment and integration of the Wazuh agent on the Windows 11 domain endpoint.

This phase includes:

- Wazuh agent installation.
- Agent configuration and Manager address assignment.
- Windows Wazuh service verification.
- Agent registration with the Wazuh Manager.
- Agent connectivity validation.
- Windows event collection.
- Wazuh alert investigation.
- Screenshot collection and technical documentation.

The Windows 11 endpoint was successfully integrated into the Wazuh SIEM environment.

The agent registered as `WIN11-CLIENT-2` and appeared as Active in the Wazuh Dashboard.

Windows event monitoring was validated through the collection and processing of a DistributedCOM event.

| Parameter | Value |
|---|---|
| Event Source | Microsoft-Windows-DistributedCOM |
| Windows Event ID | 10010 |
| Wazuh Rule ID | 61102 |
| Alert Level | 5 |

These results demonstrate that Wazuh successfully received and processed Windows endpoint telemetry from the Windows 11 client.

Documentation: `Docs/06-wazuh-agent-windows11.md`

### Phase 7 — Wazuh Agent Integration: Windows Server 2022

Documents the installation, configuration, and integration of the Wazuh agent on DC01, the Windows Server 2022 Domain Controller.

This phase includes:

- Network connectivity verification between DC01 and the Wazuh Manager.
- Wazuh Manager TCP port 1514 verification.
- TCP connectivity testing from DC01.
- Wazuh Manager service status verification.
- Wazuh agent installation through the Windows installation wizard.
- Agent configuration and Manager address assignment.
- Troubleshooting of an invalid server address.
- Correction of the `ossec.conf` configuration file.
- WazuhSvc service startup and verification.
- Agent registration with the Wazuh Manager.
- Agent status verification through the Wazuh command line.
- Agent status verification in the Wazuh Dashboard.
- Screenshot collection and technical documentation.

The Wazuh agent was installed on DC01 and configured to communicate with the Manager at `192.168.100.132`.

During configuration, the agent initially reported an invalid server address because its configuration contained `0.0.0.0`.

The issue was resolved by updating the agent configuration file:

`C:\Program Files (x86)\ossec-agent\ossec.conf`

The server address was changed to the actual Wazuh Manager IP address. The Wazuh agent service was subsequently restarted and verified as Running.

The agent was successfully registered with the Wazuh Manager. DC01 appeared as Active in both the Manager command-line output and the Wazuh Dashboard.

**Validation results:**

- Agent installation: Completed.
- Agent configuration: Completed.
- Wazuh service: Running.
- Agent registration: Successful.
- Agent connectivity: Active.
- Windows Server event collection: Validated.
- Windows Security event detection: Validated.

Documentation: `Docs/07-wazuh-agent-windows-server.md`

### Phase 8 — Windows Server Security Monitoring and Brute Force Detection

Documents the validation of Windows Server Security event collection and the development of a custom Wazuh detection rule to identify repeated failed authentication attempts.

This phase includes:

- Verification of centralized Windows Security event collection from DC01.
- Investigation of Windows Security Event ID 4625.
- Identification of failed authentication attempts.
- Analysis of target usernames and authentication details.
- Investigation of Wazuh rule 60122.
- Analysis of failed logon events through `archives.json` and `alerts.json`.
- Development of a custom Wazuh detection rule.
- Frequency-based event correlation.
- Detection of five failed logon attempts for the same username within 60 seconds.
- Assignment of a high-severity alert.
- MITRE ATT&CK mapping.
- Validation of custom rule execution.
- Investigation through the Wazuh Dashboard.
- Documentation of detection evidence.

#### Windows Event ID 4625

Windows Security Event ID `4625` is generated when an account fails to log on.

The laboratory generated controlled failed authentication attempts on DC01 using the following command:

```cmd
runas /user:ADLAB\usuario_inexistente cmd
```

The resulting events were collected by the Wazuh agent and processed by the Wazuh Manager.

The original Windows event was identified with:

| Parameter | Value |
|---|---|
| Agent ID | 002 |
| Agent Name | DC01 |
| Windows Event ID | 4625 |
| Original Wazuh Rule ID | 60122 |
| Target Username | usuario_inexistente |
| Target Domain | ADLAB |
| Logon Type | 2 |
| Source Address | ::1 |
| Failure Status | 0xc000006d |
| Failure SubStatus | 0xc0000064 |

The failure status indicates an unsuccessful authentication attempt. The substatus `0xc0000064` identifies an unknown username.

The source address `::1` represents the IPv6 loopback address, indicating that the controlled authentication attempts originated locally on DC01 rather than from a remote host.

#### Custom Wazuh Detection Rule

A custom rule was implemented in:

`/var/ossec/etc/rules/local_rules.xml`

The rule uses the original Windows failed logon rule `60122` as its matching condition.

```xml
<group name="windows,authentication_failed,brute_force,">

  <rule id="100100" level="10" frequency="5" timeframe="60">
    <if_matched_sid>60122</if_matched_sid>
    <same_field>win.eventdata.targetUserName</same_field>
    <description>Possible brute force attack: 5 failed logon attempts for the same user within 60 seconds</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>

</group>
```

**Detection logic:**

- `<if_matched_sid>60122</if_matched_sid>`: Matches failed Windows authentication events.
- `<same_field>win.eventdata.targetUserName</same_field>`: Correlates events involving the same target username.
- `frequency="5"`: Requires five matching events.
- `timeframe="60"`: Defines a 60-second correlation window.
- `level="10"`: Generates a high-severity alert.
- `T1110`: Maps the detection to MITRE ATT&CK Brute Force.

The custom rule was validated using the Wazuh analysis engine and successfully loaded by the Wazuh Manager.

#### Detection Validation

Five controlled failed authentication attempts were generated on DC01 within the configured 60-second window.

Wazuh successfully correlated the events and generated an alert with the following characteristics:

| Parameter | Result |
|---|---|
| Custom Rule ID | 100100 |
| Alert Level | 10 |
| Frequency | 5 |
| Timeframe | 60 seconds |
| Agent ID | 002 |
| Agent Name | DC01 |
| Agent IP | 192.168.100.10 |
| Rule Groups | windows, authentication_failed, brute_force |
| MITRE ATT&CK ID | T1110 |
| MITRE Tactic | Credential Access |
| MITRE Technique | Brute Force |

The alert was verified in:

`/var/ossec/logs/alerts/alerts.json`

The detection was also confirmed through the Wazuh Dashboard using the filter:

```text
rule.id: 100100 AND agent.name: DC01
```

The original failed authentication events were investigated using:

```text
agent.name: DC01 AND data.win.system.eventID: 4625 AND data.win.eventdata.targetUserName: usuario_inexistente
```

The custom alert was investigated through its rule metadata and underlying Windows event fields.

The laboratory successfully demonstrated the complete detection workflow:

1. Windows generates failed authentication events.
2. The Wazuh agent collects the events.
3. The Wazuh Manager analyzes the incoming telemetry.
4. The custom rule correlates repeated failed logons for the same username.
5. Wazuh generates a level 10 alert.
6. The alert is mapped to MITRE ATT&CK T1110.
7. The incident is investigated through the Wazuh Dashboard.

**Detection scope:** This scenario demonstrates a controlled local authentication failure test. It validates the custom correlation logic, but does not represent a real remote attack or establish that the source of the attempts was external.

Documentation: `Docs/08-wazuh-windows-security-detection.md`

## Active Directory Environment

**Domain:** `adlab.local`

**Domain Controller:** `DC01`

**Server IP:** `192.168.100.10`

**NetBIOS Domain:** `ADLAB`

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

Security groups include:

- SOC-Analysts
- AD-Admins

The laboratory uses separate administrative and standard user identities to demonstrate privilege separation and delegated administration.

## Identity and Access Management

### Standard Users

Standard user accounts are maintained in the Users-Lab OU and represent normal domain users without administrative privileges.

### SOC-Analysts

The SOC-Analysts security group represents users who require access related to Security Operations Center activities.

### AD-Admins

The AD-Admins security group represents administrators with delegated Active Directory management capabilities.

The administrative account `maria.admin` was assigned to this group.

### Delegated Administration

Administrative permissions were delegated to AD-Admins over the Users-Lab OU.

Delegated capabilities include:

- Creating, deleting, and managing user accounts.
- Resetting user passwords.
- Requiring a password change at the next logon.

The delegation was validated through a controlled password-reset operation performed by `maria.admin` against a user in Users-Lab.

This demonstrates the principle of least privilege by granting specific administrative capabilities without assigning unrestricted Domain Admin privileges.

## Group Policy Security

The workstation security baseline is managed through:

`GPO-Workstations-Security`

The GPO is linked to the Workstations OU.

The configured security controls include:

- Audit Logon.
- Successful logon auditing.
- Failed logon auditing.
- Account Lockout Policy.
- Audit Policy Change.
- PowerShell Script Block Logging.
- Microsoft Defender behavior monitoring.

The Windows 11 client was joined to `adlab.local` and placed in the Workstations OU.

After a Group Policy refresh, the client reported `GPO-Workstations-Security` in its computer policy results.

PowerShell Script Block Logging was validated by checking the registry configuration and inspecting the PowerShell Operational event log.

## Windows 11 Client Validation

The Windows 11 Pro endpoint was deployed in VMware Workstation Pro.

The following validations were completed:

- Successful installation of Windows 11 Pro.
- Network connectivity to DC01 at `192.168.100.10`.
- DNS resolution of `adlab.local`.
- Successful domain join.
- Placement of the client computer account in the Workstations OU.
- Application of GPO-Workstations-Security.
- Confirmation that `EnableScriptBlockLogging` is set to `1`.
- Observation of Event ID 4104 in `Microsoft-Windows-PowerShell/Operational`.

The VMware virtual machine is named `WIN11-CLIENT-2`. The computer account registered in Active Directory is `DESKTOP-HGSPMK5`.

The Windows 11 endpoint has also been integrated with Wazuh. Its agent is active and Windows event detection has been validated.

## Wazuh SIEM/XDR Integration

Wazuh has been deployed on an Ubuntu Server 24.04 LTS virtual machine using an All-in-One architecture.

The deployment includes:

- Wazuh Indexer.
- Wazuh Manager.
- Wazuh Dashboard.

**Wazuh Manager IP address:** `192.168.100.132`

The Dashboard is accessible, and both Windows systems have been configured as Wazuh agents.

### Integrated Endpoints

| Endpoint | Operating System | Agent ID | Agent Status |
|---|---|---|---|
| WIN11-CLIENT-2 | Windows 11 Pro | Previously registered | Active |
| DC01 | Windows Server 2022 | 002 | Active |

### Windows 11 Integration Results

- Agent installed and configured.
- Agent registered with Wazuh Manager.
- Agent status verified as Active.
- Windows event collection validated.
- DistributedCOM event processed by Wazuh.
- Detection evidence documented.

### Windows Server 2022 Integration Results

- Agent installed and configured.
- Invalid server address issue resolved.
- WazuhSvc service verified as Running.
- Agent registered with Wazuh Manager.
- Agent status verified as Active.
- Windows Security event collection validated.
- Event ID 4625 successfully collected.
- Original authentication failure rule 60122 investigated.
- Custom brute force detection rule 100100 implemented and validated.
- High-severity alert generated and investigated through the Dashboard.

## Lab Architecture

```text
                 Active Directory Domain
                      adlab.local
                           |
                +----------+----------+
                |                     |
                v                     v
               DC01              Windows 11
        Windows Server 2022     WIN11-CLIENT-2
                |                     |
           Wazuh Agent           Wazuh Agent
                |                     |
                +----------+----------+
                           |
                           v
                    Wazuh Manager
                    Ubuntu Server
                    192.168.100.132
                           |
                           v
                     Wazuh Indexer
                           |
                           v
                    Wazuh Dashboard
                           |
                           v
                 Detection and Analysis
                           |
                           v
                 Security Investigation
```

Both Windows agents are registered and active in the Wazuh environment. Windows 11 event detection and DC01 Windows Security event collection have been validated.

## Virtualization and Networking

The Windows Server environment is hosted in VMware Workstation Pro using a NAT network configuration.

The Windows 11 client uses DC01 as its preferred DNS server to resolve the Active Directory domain.

The Wazuh server runs on a separate Ubuntu Server virtual machine.

Network communication between the SIEM and Windows systems is required for centralized security monitoring.

The lab is intended for controlled testing and defensive security exercises, separate from production systems.

## Security Focus

The laboratory incorporates defensive security practices, including:

- Active Directory security.
- Identity and access management.
- Least privilege.
- Privileged account separation.
- Group Policy security.
- Windows endpoint hardening.
- Firewall configuration.
- Event logging and auditing.
- PowerShell script logging.
- Authentication monitoring.
- Account lockout protection.
- Microsoft Defender endpoint protection.
- Active Directory security event analysis.
- Centralized log collection.
- SIEM monitoring.
- Detection engineering.
- Frequency-based event correlation.
- MITRE ATT&CK mapping.
- Threat hunting.
- Security event investigation.

## Evidence

Screenshots documenting significant configuration and validation steps are stored in the `Screenshots/` directory.

Evidence includes:

- Windows Server installation.
- Hostname and static IP configuration.
- Active Directory Domain Services installation.
- Domain Controller promotion.
- Active Directory domain configuration.
- Organizational Unit structure.
- User and security group creation.
- Delegated administration configuration.
- Group Policy creation and linkage.
- Audit Logon configuration.
- Account Lockout configuration.
- Audit Policy Change configuration.
- PowerShell Script Block Logging configuration.
- Microsoft Defender behavior monitoring configuration.
- Windows 11 virtual machine configuration and installation.
- Connectivity between Windows 11 and DC01.
- DNS resolution for the domain.
- Windows 11 domain join.
- Workstation GPO application.
- PowerShell Script Block Logging registry configuration.
- PowerShell Operational Event ID 4104.
- Wazuh agent installation on Windows 11.
- Windows 11 Wazuh service verification.
- Windows 11 agent registration and connectivity.
- Windows 11 event detection in Wazuh.
- Connectivity between DC01 and the Wazuh Manager.
- Wazuh Manager TCP port 1514 verification.
- DC01 TCP connectivity test.
- Wazuh Manager service status.
- Wazuh agent installation wizard on DC01.
- DC01 agent registration and active status.
- DC01 agent visibility in the Wazuh Dashboard.
- DC01 Windows Security Event ID 4625 collection.
- Failed authentication event investigation.
- Custom Wazuh rule 100100 configuration.
- Five failed logon attempts correlated within 60 seconds.
- High-severity brute force detection alert.
- MITRE ATT&CK T1110 mapping.
- Wazuh Dashboard investigation and alert verification.

The screenshots provide supporting evidence for the steps described in the phase documentation.

Sensitive information such as passwords, credentials, tokens, private keys, and other secrets must not be committed to the repository.

## Status

**Current status:** Active Directory and the Windows 11 domain client are configured. Workstation Group Policy application and PowerShell Script Block Logging have been validated locally. Wazuh is deployed on Ubuntu Server. Both Windows endpoints are registered as active Wazuh agents.

Windows 11 event detection has been validated. DC01 Windows Security event collection and custom brute force detection have also been successfully validated.

### Completed

- VMware virtual machine created.
- Windows Server 2022 Standard installed.
- Desktop Experience installed.
- Administrator account configured.
- Server hostname configured.
- Static IP and DNS configured.
- Active Directory Domain Services installed.
- Domain Controller promotion completed.
- `adlab.local` forest and domain configured.
- NetBIOS domain name configured.
- DNS resolution verified.
- Organizational Units created.
- Standard user account created.
- SOC-Analysts security group created.
- Privileged administrative account created.
- AD-Admins security group created.
- Delegated administration configured and validated.
- GPO-Workstations-Security created.
- GPO linked to the Workstations OU.
- Audit Logon configured.
- Successful and failed logon auditing enabled.
- Account Lockout Policy configured.
- Account lockout threshold set to 5 attempts.
- Account lockout duration set to 15 minutes.
- Account lockout counter reset configured for 15 minutes.
- Audit Policy Change configured.
- PowerShell Script Block Logging configured.
- Microsoft Defender behavior monitoring configured.
- Windows 11 Pro client deployed.
- Windows 11 connectivity to DC01 verified.
- DNS resolution for `adlab.local` verified.
- Windows 11 client joined to the domain.
- Client computer account moved to the Workstations OU.
- Workstation GPO application verified.
- PowerShell Script Block Logging registry value verified.
- PowerShell Operational Event ID 4104 observed.
- Phase 5 documentation and screenshots collected.
- Wazuh All-in-One deployed on Ubuntu Server.
- Wazuh Dashboard accessed successfully.
- Windows 11 Wazuh agent installed and configured.
- Windows 11 agent registered and verified as Active.
- Windows 11 event detection validated.
- DC01 Wazuh agent installed and configured.
- DC01 invalid server address configuration corrected.
- DC01 WazuhSvc service verified as Running.
- DC01 agent registered and verified as Active.
- DC01 agent status confirmed through the Wazuh Manager and Dashboard.
- DC01 Windows Security event collection validated.
- Windows Security Event ID 4625 collected and investigated.
- Wazuh rule 60122 identified and analyzed.
- Custom Wazuh rule 100100 implemented.
- Five failed authentication attempts correlated within 60 seconds.
- Custom brute force detection successfully triggered.
- High-severity level 10 alert generated.
- MITRE ATT&CK T1110 mapping validated.
- Custom alert groups and description corrected and verified.
- Brute force alert investigated through the Wazuh Dashboard.
- Phase 8 detection validation completed.

### In Progress / Planned

- Complete and publish Phase 8 technical documentation.
- Expand Windows Server security event monitoring.
- Validate PowerShell event collection from DC01.
- Generate controlled account-management events.
- Monitor user creation and deletion.
- Monitor password resets and group membership changes.
- Develop additional custom Wazuh detection rules.
- Investigate additional Active Directory security scenarios.
- Expand threat hunting and detection engineering capabilities.

## Future Lab Development

Future stages will include:

- Domain authentication testing.
- Additional Group Policy validation.
- Security auditing validation.
- Controlled security scenarios.
- Active Directory monitoring.
- User and group management event detection.
- Privilege escalation monitoring.
- PowerShell activity monitoring on DC01.
- Additional custom detection rules.
- Threat hunting.
- Security event investigation.
- Documentation of detection use cases.

The final objective is to demonstrate an end-to-end defensive security workflow:

```text
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
     |
     v
Documentation
```

## Remote Administration with PowerShell Remoting

PowerShell Remoting was previously configured and validated for remote access to the Windows Server 2022 Domain Controller.

### Environment

- Domain Controller: DC01
- Domain: adlab.local
- Server IP: 192.168.100.10
- Remote management: PowerShell Remoting (WinRM)

### Connection Example

```powershell
$cred = Get-Credential
Enter-PSSession -ComputerName 192.168.100.10 -Credential $cred
```

### Validation Example

```powershell
hostname
Get-Service DNS, NTDS, WinRM
```

The remote session was used to confirm the server hostname and inspect the status of DNS, Active Directory Domain Services (NTDS), and WinRM.

Remote administration should be restricted to authorized accounts and trusted hosts. Production environments require appropriate authentication, access restrictions, and transport security.

## Disclaimer

This project is intended for educational and defensive cybersecurity purposes.

All security testing and experimentation is performed within a controlled laboratory environment owned or administered by the author.

No production systems or third-party environments are intentionally targeted as part of this project.