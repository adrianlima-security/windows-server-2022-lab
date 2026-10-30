# Wazuh Agent – Windows 11 Endpoint Integration

## 1. Overview

This phase documents the deployment and integration of the Wazuh agent on a Windows 11 endpoint.

The objective is to connect the Windows 11 machine to the Wazuh Manager, verify agent connectivity, and validate the collection of Windows security events.

## 2. Lab Environment

| Component        | Configuration   |
| ---------------- | --------------- |
| Operating System | Windows 11      |
| Agent Name       | WIN11-CLIENT-2  |
| Agent ID         | 001             |
| Agent IP         | 192.168.100.130 |
| Wazuh Manager    | Ubuntu Server   |
| Manager IP       | 192.168.100.132 |
| SIEM Platform    | Wazuh           |

## 3. Wazuh Agent Installation

The Wazuh agent was installed on the Windows 11 endpoint using the official Windows installer.

The installation process included configuring the Wazuh Manager address and registering the endpoint.

**Screenshot 37:** Wazuh Agent installation command.

![Wazuh Agent Installation](screenshots/37-wazuh-agent-install-command.png)

## 4. Windows Agent Service Verification

After installation, the Wazuh agent service was checked using PowerShell.

The service was verified to ensure that the agent was running correctly.

Service name:

`WazuhSvc`

**Screenshot 38:** Wazuh Agent service running.

![Wazuh Agent Service](screenshots/38-wazuh-agent-service-running.png)

## 5. Agent Registration and Connectivity

The Windows 11 endpoint was successfully registered with the Wazuh Manager.

The agent appeared in the Wazuh Dashboard with the following information:

- Agent Name: WIN11-CLIENT-2
- Agent ID: 001
- Status: Active
- IP Address: 192.168.100.130

**Screenshot 39:** Windows 11 agent active in Wazuh Dashboard.

![Wazuh Agent Dashboard](screenshots/39-wazuh-agent-active-dashboard.png)

## 6. Windows Event Monitoring and Detection

The endpoint integration was validated by generating and collecting real Windows events.

Wazuh successfully received a Windows event from:

`Microsoft-Windows-DistributedCOM`

Detection details:

| Parameter     | Value                            |
| ------------- | -------------------------------- |
| Event ID      | 10010                            |
| Wazuh Rule ID | 61102                            |
| Alert Level   | 5                                |
| Event Source  | Microsoft-Windows-DistributedCOM |

This confirmed that the Wazuh Manager was receiving Windows endpoint telemetry and processing events through its detection rules.

**Screenshot 40:** Windows 11 event detection in Wazuh Dashboard.

![Windows 11 Event Detection](screenshots/40-wazuh-windows11-event-detection.png)

## 7. Results

The Windows 11 endpoint was successfully integrated into the Wazuh SIEM environment.

The following objectives were achieved:

- Wazuh agent installation and configuration.
- Windows service verification.
- Successful agent registration.
- Active communication with the Wazuh Manager.
- Collection and detection of real Windows events.

## 8. Conclusion

The integration of the Windows 11 endpoint established the foundation for centralized security monitoring.

The endpoint is now part of the Wazuh SIEM Home Lab, enabling Windows event collection, security monitoring, and further investigation of potential security incidents.

**Next Phase:** Wazuh Agent Integration – Windows Server 2022 (DC01).