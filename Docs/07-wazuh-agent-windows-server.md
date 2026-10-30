# Wazuh Agent Integration – Windows Server 2022 (DC01)

## 1. Overview

This document describes the installation, configuration, and integration of the Wazuh agent on the Windows Server 2022 virtual machine acting as the Domain Controller (DC01).

The objective is to connect the Windows Server endpoint to the centralized Wazuh Manager and verify that the agent is successfully registered and active.

## 2. Lab Environment

| Component          | Configuration           |
| ------------------ | ----------------------- |
| Endpoint           | Windows Server 2022     |
| Hostname           | DC01                    |
| Domain             | adlab.local             |
| DC01 IP Address    | 192.168.100.10          |
| Wazuh Manager      | Ubuntu Server 24.04 LTS |
| Wazuh Manager IP   | 192.168.100.132         |
| Agent Service      | WazuhSvc                |
| Communication Port | TCP 1514                |

## 3. Network Connectivity Verification

Before installing and configuring the agent, network connectivity between DC01 and the Wazuh Manager was verified.

The following checks were performed:

- ICMP connectivity between DC01 and the Wazuh Manager.
- Verification that the Wazuh Manager was listening on TCP port 1514.
- TCP connectivity test from DC01 to the Wazuh Manager.

The TCP connectivity test returned `TcpTestSucceeded: True`, confirming that DC01 could reach the Wazuh Manager on port 1514.

**Screenshot 41 – DC01 Connectivity to Wazuh Manager**

![DC01 Connectivity to Wazuh Manager](../Screenshots/41-dc01-connectivity-to-wazuh.png)

**Screenshot 42 – Wazuh Manager Listening on TCP Port 1514**

![Wazuh Manager Port 1514](../Screenshots/42-wazuh-manager-port-1514.png)

**Screenshot 43 – DC01 TCP Connectivity Test**

![DC01 TCP Connectivity](../Screenshots/43-dc01-tcp-connectivity-1514.png)

**Screenshot 44 – Wazuh Manager Service Status**

![Wazuh Manager Service Status](../Screenshots/44-wazuh-manager-service-status.png)

## 4. Wazuh Agent Installation

The Wazuh agent was installed on Windows Server 2022 (DC01) using the Windows installation wizard.

The installation was performed on the Domain Controller to enable centralized endpoint monitoring through the Wazuh platform.

**Screenshot 45 – Wazuh Agent Installation Wizard**

![Wazuh Agent Installation Wizard](../Screenshots/45-wazuh-agent-installation-wizard.png)

## 5. Agent Configuration

After installation, the agent configuration file was reviewed:

`C:\Program Files (x86)\ossec-agent\ossec.conf`

The initial configuration contained an invalid server address:

`0.0.0.0`

This caused the agent to fail when attempting to connect to the Wazuh Manager.

The configuration was corrected to point to the actual Wazuh Manager IP address:

```xml
<client>
    <server>
        <address>192.168.100.132</address>
    </server>
</client>
```

This change ensured that the agent used the correct destination address.

## 6. Wazuh Agent Service Verification

After correcting the configuration, the Wazuh agent service was restarted using PowerShell:

```powershell
Stop-Service WazuhSvc
Start-Service WazuhSvc
Start-Sleep -Seconds 3
Get-Service WazuhSvc
```

The service status was verified as `Running`.

## 7. Agent Registration and Status Verification

The agent status was verified from the Wazuh Manager using the following command:

```bash
sudo /var/ossec/bin/agent_control -l
```

The output confirmed that DC01 was successfully registered and displayed an `Active` status.

The Wazuh Dashboard was also checked to confirm that the endpoint appeared with the following information:

- Agent name: DC01
- IP address: 192.168.100.10
- Status: Active

**Screenshot 46 – DC01 Agent Active on Wazuh Manager**

![DC01 Agent Active on Wazuh Manager](../Screenshots/46-dc01-agent-active-wazuh-manager.png)

**Screenshot 47 – DC01 Agent Active on Wazuh Dashboard**

![DC01 Agent Active on Wazuh Dashboard](../Screenshots/47-dc01-agent-active-dashboard.png)

## 8. Integration Status

The integration of the Wazuh agent on Windows Server 2022 was successfully completed.

The following tasks were verified:

- Wazuh agent installed on DC01.
- Agent configuration updated with the correct Wazuh Manager IP address.
- Wazuh agent service running successfully.
- Agent registered with the Wazuh Manager.
- DC01 displayed as `Active` in the Wazuh Manager.
- DC01 displayed as `Active` in the Wazuh Dashboard.

**Pending validation:**

The collection and detection of Windows Server security events still need to be tested and validated.

## 9. Next Steps

The next phase will focus on validating security event collection from DC01, including:

- Windows Security Event Log monitoring.
- Active Directory authentication events.
- Account and privilege-related activity.
- Security event visibility in the Wazuh Dashboard.
- Detection rules and alert generation.

This will extend the lab from basic agent connectivity to centralized security monitoring and threat detection across both Windows endpoints.