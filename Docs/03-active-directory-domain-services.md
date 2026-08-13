# 03 — Active Directory Domain Services

## Overview

This phase documents the deployment and initial configuration of Active Directory Domain Services (AD DS) on the Windows Server 2022 virtual machine.

The server was configured as a Domain Controller for the laboratory domain: adlab.local

The goal of this phase was to establish a functional Active Directory environment that can later be used for identity management, access control, security monitoring, and detection engineering with Wazuh.

---

## 3.1 Initial Server Configuration

Before installing Active Directory Domain Services, the Windows Server environment was configured with the required system and network settings.

### Server Hostname

The default Windows Server hostname was changed to:

DC01

The hostname was selected to clearly identify the server as the primary Domain Controller of the laboratory environment.

### Evidence

The configured hostname is documented in:

../Screenshots/02-hostname-configured.png

---

## 3.2 Static IP Configuration

A static IPv4 configuration was assigned to the server.

Configuration:

IPv4 Address: 192.168.100.128
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.100.2

The server was configured with a static IP because a Domain Controller should maintain a predictable network address.

The static address also provides a stable destination for DNS and other domain services.

### Evidence

The static IP configuration is documented in:

../Screenshots/03-static-ip-configuration.png

---

## 3.3 DNS Configuration

The initial DNS configuration used:

DNS Server: 192.168.100.2

After Active Directory Domain Services was deployed and the server was promoted to a Domain Controller, DNS functionality was integrated with the Active Directory environment.

DNS is a critical component of Active Directory because domain clients use DNS to locate Domain Controllers and other domain services.

---

## 3.4 Active Directory Domain Services Installation

The Active Directory Domain Services (AD DS) server role was installed through Server Manager.

The installation completed successfully on DC01.

### Evidence

The selected AD DS role is documented in:

../Screenshots/04-ad-ds-role-selected.png

The successful installation is documented in:

../Screenshots/05-ad-ds-installation-complete.png

---

## 3.5 Domain Controller Promotion

After installing the AD DS role, the server was promoted to a Domain Controller.

A new Active Directory forest was created using:

adlab.local

This established the laboratory Active Directory domain.

### Domain Controller Configuration

The server was configured as the first Domain Controller in the new forest.

The NetBIOS domain name was configured as:

ADLAB

The resulting domain authentication format is:

ADLAB\username

### Evidence

The new forest configuration is documented in:

../Screenshots/06-new-forest-adlab.png

The Domain Controller configuration is documented in:

../Screenshots/07-domain-controller-options.png

The NetBIOS domain name configuration is documented in:

../Screenshots/08-netbios-domain-name.png

The promotion review configuration is documented in:

../Screenshots/09-ad-ds-review-options.png

After the promotion and restart, the server successfully accepted domain authentication using:

ADLAB\Administrator

### Evidence

The Domain Controller login is documented in:

../Screenshots/10-domain-controller-login.png

---

## 3.6 Active Directory and DNS Verification

After Domain Controller promotion, the Active Directory domain was verified from the server.

The domain was confirmed as:

adlab.local

DNS resolution was also tested using the following commands:

nslookup adlab.local

nslookup dc01.adlab.local

Both queries successfully resolved through the laboratory DNS configuration.

This confirmed that the Active Directory domain and DNS services were functioning correctly.

### Evidence

The Active Directory domain structure is documented in:

../Screenshots/11-active-directory-domain.png

---

## 3.7 Organizational Unit Structure

Organizational Units (OUs) were created to provide a logical structure for the laboratory environment.

The structure separates users, groups, administrative accounts, servers, and workstations.

The resulting structure includes:

adlab.local
├── Admins
├── Groups
├── Users-Lab
├── Servers
└── Workstations

Organizational Units provide administrative and policy boundaries within Active Directory.

They will also be used later when applying Group Policy Objects (GPOs) and delegating administrative permissions.

### Evidence

The Active Directory OU structure is documented in:

../Screenshots/12-active-directory-ous.png

---

## 3.8 User Accounts

A standard laboratory user account was created inside:

Users-Lab

The account represents a normal domain user without administrative privileges.

The purpose of separating standard users from administrative accounts is to demonstrate the principle of least privilege.

### Evidence

The creation of the laboratory user account is documented in:

../Screenshots/13-ad-user-created.png

---

## 3.9 Security Groups

A security group was created for users performing SOC-related tasks:

SOC-Analysts

Configuration:

Group Type: Security
Group Scope: Global

The group is intended to represent users who require specific permissions or access related to SOC activities.

Using a security group allows permissions to be assigned to a role rather than individually assigning permissions to every user.

This makes the environment easier to manage and scale.

### Evidence

The SOC-Analysts group configuration is documented in:

../Screenshots/15-soc-analysts-group.png

User group membership was also verified during the configuration process.

### Evidence

../Screenshots/14-user-group-membership.png

---

## 3.10 Privileged Administrative Account

A separate administrative account was created:

maria.admin

The account was placed inside:

Admins

The purpose of this account is to demonstrate the separation between normal user identities and privileged administrative identities.

The account initially belonged to the default Domain Users group and was not assigned Domain Admin privileges.

This separation follows the principle of least privilege and reduces the need to use highly privileged accounts for routine administrative tasks.

---

## 3.11 AD-Admins Security Group

A dedicated security group was created:

AD-Admins

Configuration:

Group Type: Security
Group Scope: Global

The group is intended to represent administrators who require delegated Active Directory management capabilities.

The administrative account maria.admin was assigned to this group.

The purpose of using a group instead of assigning permissions directly to individual users is to provide a scalable administrative model.

For example, additional administrators can later be added to AD-Admins without modifying the delegated permissions themselves.

---

## 3.12 Delegated Administration

Administrative permissions were delegated to the AD-Admins security group over the Users-Lab Organizational Unit.

The delegation was intentionally limited to specific user-management tasks rather than assigning Domain Admin privileges.

The following permissions were delegated:

Create, delete, and manage user accounts

Reset user passwords and force password change at next logon

This configuration demonstrates the principle of least privilege.

Instead of granting unrestricted administrative access to the domain, the administrative group receives only the permissions required to perform its intended tasks within the relevant OU.

### Evidence

The delegated permissions are documented in:

../Screenshots/16-ad-delegation-users-lab.png

---

## 3.13 Delegation Validation

The delegated permissions were validated using the administrative account:

ADLAB\maria.admin

The Active Directory Users and Computers console was executed using the maria.admin identity.

A controlled password-reset operation was then performed against the standard user:

Maria Garcia

located inside:

Users-Lab

The password reset completed successfully.

This validated that maria.admin, through membership in AD-Admins, was able to perform the delegated password-management operation against users within the Users-Lab OU.

No Domain Admin membership was required to perform this operation.

> Note: The validation was successfully performed in the laboratory environment, but no separate screenshot was captured for the successful password-reset operation.

---

## Security Concepts Demonstrated

This phase introduced several important Active Directory and security concepts:

### Least Privilege

Administrative users should receive only the permissions required for their responsibilities.

### Role-Based Access Control

Security groups can be used to represent roles and assign permissions to groups instead of individual users.

### Privileged Account Separation

Administrative activities should be performed using separate privileged identities rather than using highly privileged accounts for normal activities.

### Delegated Administration

Active Directory permissions can be delegated to specific groups over specific Organizational Units.

### Defense in Depth

Limiting administrative permissions reduces the potential impact of a compromised privileged account.

---

## Current Laboratory Architecture

The current Active Directory environment can be represented as:

adlab.local
│
▼
DC01
Windows Server 2022
│
├── Users-Lab
│   └── Standard Users
│
└── Groups
    ├── SOC-Analysts
    └── AD-Admins
        └── maria.admin

The Active Directory environment is now ready for the next stage of the laboratory.

---

## Next Phase

The next stage will focus on Group Policy Objects (GPOs) and security-related configuration.

The laboratory will later be extended with a Windows 11 client that will join the adlab.local domain.

This will allow the laboratory to demonstrate:

* Domain authentication
* Group Policy enforcement
* User and computer management
* Security event generation
* Authentication monitoring
* Privileged activity
* PowerShell activity
* Active Directory security monitoring
* Wazuh-based detection and analysis