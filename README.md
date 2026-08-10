# Windows Server 2022 Cybersecurity Lab

## Overview

This project documents the deployment, configuration, and security hardening of a Windows Server 2022 virtual machine as part of a personal cybersecurity home lab.

The objective is to build a controlled Windows Server environment that can be used to practice system administration, networking, identity management, security monitoring, detection engineering, and defensive security techniques.

The entire environment is documented to provide a reproducible laboratory and demonstrate practical cybersecurity skills.

---

## Objectives

The main objectives of this laboratory are:

* Deploy Windows Server 2022 in a virtualized environment.
* Configure and administer a Windows Server system.
* Establish a secure baseline configuration.
* Configure networking and Windows security controls.
* Implement identity and access management concepts.
* Configure logging and security monitoring.
* Generate and investigate security events.
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

---

## Project Structure

```text
Windows-Server-2022-Lab/
│
├── README.md
│
├── docs/
│   ├── 01-vm-creation.md
│   └── 02-windows-server-installation.md
│
└── screenshots/
    └── 01-first-server-desktop.png
```

Additional documentation and evidence will be added as the laboratory progresses.

---

## Documentation

### Phase 1 — Virtual Machine Creation

Documents the creation and configuration of the VMware virtual machine, including CPU, memory, storage, networking, installation media, and initial boot troubleshooting.

→ [`01-vm-creation.md`](docs/01-vm-creation.md)

### Phase 2 — Windows Server Installation

Documents the installation of Windows Server 2022 Standard with Desktop Experience, including disk selection, installation type, administrator configuration, and first successful boot.

→ [`02-windows-server-installation.md`](docs/02-windows-server-installation.md)

### Phase 3 — Initial Server Configuration

Planned documentation covering the initial server baseline, hostname, networking, Windows Update, firewall, users, and system configuration.

---

## Virtualization

The Windows Server environment is hosted in VMware Workstation Pro.

The virtual machine currently uses a NAT network configuration, providing Internet connectivity through the host system while avoiding direct exposure of the server to the physical network.

---

## Security Focus

The laboratory will progressively incorporate defensive security practices including:

* Windows security configuration
* Firewall configuration
* User and privilege management
* Event logging
* Security auditing
* PowerShell logging
* System monitoring
* Detection engineering
* Security event investigation
* Hardening techniques

The environment is designed to remain isolated and controlled so that security testing can be performed safely.

---

## Evidence

Screenshots documenting significant configuration and testing steps are stored in the `screenshots/` directory.

Screenshots are used as supporting evidence for the configuration and implementation described in the documentation.

Sensitive information such as passwords, credentials, tokens, or other secrets will not be stored in the repository.

---

## Status

**Current status:** Windows Server 2022 successfully installed.

### Completed

* [x] VMware virtual machine created
* [x] Windows Server 2022 Evaluation ISO configured
* [x] VM boot troubleshooting completed
* [x] Windows Server 2022 Standard installed
* [x] Desktop Experience installed
* [x] Administrator account configured
* [x] First successful boot completed
* [x] Initial documentation created

### In Progress

* [ ] Establish Windows Server baseline
* [ ] Configure hostname
* [ ] Configure network settings
* [ ] Install Windows updates
* [ ] Review firewall configuration
* [ ] Configure security auditing
* [ ] Implement additional security controls
* [ ] Develop detection and monitoring use cases

---

## Disclaimer

This project is intended for educational and defensive cybersecurity purposes.

All security testing and experimentation is performed within a controlled laboratory environment owned or administered by the author.