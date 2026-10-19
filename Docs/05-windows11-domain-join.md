05 - Windows 11 Domain Join and Group Policy Verification

1. Objective

Deploy a Windows 11 client, connect it to the Windows Server 2022 Active Directory domain, verify DNS resolution and network connectivity, and validate the application of workstation security policies.

2. Windows 11 Client Configuration

A Windows 11 Pro virtual machine was created in VMware Workstation Pro.

The installation was completed using a manually configured virtual machine with the Windows 11 ISO mounted through the virtual CD/DVD drive.

Evidence: Screenshots/23-win11-client-vm-configuration.png and Screenshots/24-windows11-client-installation.png

3. Network Connectivity and DNS

The domain controller, DC01, uses the IP address 192.168.100.10.

The Windows 11 client was configured to use 192.168.100.10 as its preferred DNS server. Network connectivity to the domain controller was tested successfully, and DNS resolution for adlab.local was verified.

Evidence:

Screenshots/25-win11-dc01-ping.png

Screenshots/26-win11-domain-dns-resolution.png

4. Active Directory Domain Join

The Windows 11 client was successfully joined to the adlab.local domain.

The computer account was identified as DESKTOP-HGSPMK5 in Active Directory Users and Computers and moved to the Workstations organizational unit.

Evidence: Screenshots/27-win11-domain-join-success.png

5. Group Policy Verification

After moving the computer account to the Workstations OU, the Group Policy update was performed.

The GPO-Workstations-Security policy appeared in the computer's Group Policy results, confirming that the workstation security GPO was applied.

Evidence: Screenshots/28-win11-applied-group-policy.png

6. PowerShell Script Block Logging

The registry configuration was checked to verify that PowerShell Script Block Logging was enabled.

The following registry value was confirmed:

Registry path: HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging

Value: EnableScriptBlockLogging

Data: 1

Evidence: Screenshots/29-win11-powershell-logging.png

7. Event Log Verification

The Microsoft-Windows-PowerShell/Operational event log was inspected using Event Viewer.

Recent events with Event ID 4104 were found, confirming that PowerShell script block logging events were being generated on the Windows 11 client.

Evidence: Screenshots/30-win11-powershell-event-4104.png

8. Outcome

The Windows 11 client was successfully connected to the Active Directory domain. DNS resolution and connectivity to the domain controller were verified, the workstation security GPO was applied, and PowerShell Script Block Logging was validated through the Windows Event Viewer.

9. Next Steps

Integrate the Windows 11 client with the Wazuh SIEM.

Verify that Windows security and PowerShell events are collected centrally.

Use the collected telemetry to support security monitoring and detection exercises.