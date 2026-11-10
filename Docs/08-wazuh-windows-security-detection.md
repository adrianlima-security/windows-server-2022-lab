# 08 – Wazuh Windows Security Event Monitoring and Brute-Force Detection

## Overview

This phase focuses on centralizing Windows Security events from the Domain Controller (DC01) in Wazuh and implementing a custom correlation rule to identify repeated failed logon attempts.

The objective is to demonstrate basic detection engineering: collecting endpoint telemetry, investigating security events, creating a custom detection rule, and validating the resulting alert in the Wazuh Dashboard.

## Lab Environment

| Component               | Configuration                |
| ----------------------- | ---------------------------- |
| Domain Controller       | DC01                         |
| Operating System        | Windows Server 2022 Standard |
| Active Directory Domain | `adlab.local`                |
| DC01 IP Address         | `192.168.100.10`             |
| Wazuh Manager           | Ubuntu Server 24.04 LTS      |
| Wazuh Manager IP        | `192.168.100.132`            |
| Wazuh Agent ID          | `002`                        |
| Monitored Windows Event | Event ID `4625`              |

## 1. Verify Wazuh Agent Connectivity

Before configuring detection rules, the DC01 agent was verified in the Wazuh environment.

The following checks confirmed that the Windows agent service was running and that the endpoint was communicating with the Wazuh Manager.

- Wazuh agent service: Running
- Agent name: DC01
- Agent ID: 002
- Agent status: Active
- Manager address: `192.168.100.132`

The agent configuration was also corrected to use the actual Wazuh Manager IP address instead of the invalid `0.0.0.0` address.

## 2. Generate Controlled Failed Logon Events

To validate Windows Security event collection, multiple failed authentication attempts were generated on DC01 using a deliberately nonexistent domain account.

The following command was used:

```powershell
runas /user:ADLAB\usuario_inexistente cmd
```

Five failed authentication attempts were generated in a controlled lab environment.

This test produced Windows Security Event ID `4625`, which records failed logon attempts.

## 3. Investigate Windows Security Event ID 4625

The events were verified in the Windows Security log and subsequently confirmed in Wazuh.

Relevant event information included:

| Field           | Observed value                    |
| --------------- | --------------------------------- |
| Event ID        | `4625`                            |
| Target username | `usuario_inexistente`             |
| Target domain   | `ADLAB`                           |
| Logon type      | `2`                               |
| Status          | `0xc000006d`                      |
| Substatus       | `0xc0000064`                      |
| Workstation     | `DC01`                            |
| IP address      | `::1`                             |
| Caller process  | `C:\Windows\System32\svchost.exe` |

The substatus `0xc0000064` indicates that the specified user account does not exist.

The IP address `::1` represents the IPv6 loopback address. Therefore, these events were generated locally on the Domain Controller and should not be interpreted as evidence of an external attack.

## 4. Verify the Default Wazuh Detection

Wazuh successfully processed the failed logon events using its built-in detection rule.

| Property       | Value                 |
| -------------- | --------------------- |
| Rule ID        | `60122`               |
| Severity level | `5`                   |
| Event ID       | `4625`                |
| Description    | Windows logon failure |

The events were verified in Wazuh archives and alerts, confirming that DC01 security telemetry was reaching the SIEM and being analyzed.

## 5. Create a Custom Brute-Force Detection Rule

A custom Wazuh rule was implemented to identify repeated failed logon attempts for the same username.

The rule correlates five matching failed authentication events within a 60-second window.

**Custom rule configuration:**

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

### Detection logic

| Parameter         | Configuration                  |
| ----------------- | ------------------------------ |
| Custom rule ID    | `100100`                       |
| Severity level    | `10`                           |
| Reference rule    | `60122`                        |
| Frequency         | `5` events                     |
| Timeframe         | `60` seconds                   |
| Correlation field | `win.eventdata.targetUserName` |
| MITRE ATT&CK      | `T1110 – Brute Force`          |

The `same_field` condition ensures that the failed logon events are associated with the same target username.

This helps identify repeated authentication failures against a particular account rather than simply counting unrelated failed logons.

## 6. Validate the Custom Detection Rule

After creating the custom rule, the Wazuh configuration was validated using:

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
```

The validation completed successfully.

The Wazuh Manager was then restarted to load the updated configuration:

```bash
sudo systemctl restart wazuh-manager
```

The manager service status was checked:

```bash
sudo systemctl status wazuh-manager
```

The service was confirmed as active.

## 7. Confirm the Correlation Alert

The five controlled failed logon attempts triggered the custom detection rule.

The resulting alert contained the following information:

| Property        | Observed value                    |
| --------------- | --------------------------------- |
| Rule ID         | `100100`                          |
| Severity level  | `10`                              |
| Agent           | `DC01`                            |
| Agent ID        | `002`                             |
| Agent IP        | `192.168.100.10`                  |
| Detection       | Possible brute-force pattern      |
| Correlation     | 5 failed logons within 60 seconds |
| MITRE technique | `T1110`                           |

The alert was also confirmed in the Wazuh Dashboard.

The custom rule was assigned the groups:

- `windows`
- `authentication_failed`
- `brute_force`

This confirms that the detection was successfully generated, classified, and displayed in the SIEM.

## 8. Results

The following objectives were completed:

- Verified DC01 agent connectivity with the Wazuh Manager.
- Confirmed centralized collection of Windows Security events.
- Identified and investigated Event ID `4625`.
- Verified the default Wazuh detection rule `60122`.
- Created custom correlation rule `100100`.
- Configured detection of five failed logon attempts within 60 seconds for the same username.
- Mapped the detection to MITRE ATT&CK technique `T1110`.
- Validated the Wazuh configuration and restarted the Manager.
- Confirmed the custom level 10 alert in the Wazuh Dashboard.

## Conclusion

This phase demonstrates the implementation of a basic SIEM detection workflow using Wazuh and Windows Server 2022.

By collecting Windows Security events, investigating failed authentication attempts, and creating a custom correlation rule, the lab demonstrates how endpoint telemetry can be transformed into actionable security alerts.

The detection was validated using controlled authentication failures in the isolated lab environment. It represents a possible brute-force pattern and does not establish that an actual attack occurred.

## Future Improvements

Potential extensions to this detection scenario include:

- Detecting failed logons across multiple user accounts.
- Monitoring successful logons following repeated failures.
- Investigating Windows Event ID `4624` and `4648`.
- Creating additional custom rules for suspicious account activity.
- Testing authentication events from the Windows 11 domain-joined client.
- Developing alert thresholds and response procedures for different authentication scenarios.