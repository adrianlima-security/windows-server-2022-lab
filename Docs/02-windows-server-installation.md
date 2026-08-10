# 02 — Windows Server 2022 Installation

## Overview

This phase documents the installation of Windows Server 2022 on the virtual machine created in the previous phase.

The installation was performed using the official Windows Server 2022 Evaluation ISO mounted through VMware Workstation Pro.

## Installation Media

The installation used the following ISO:

```text
SERVER_EVAL_X64FRE_en-us.iso
```

Configuration:

* **Language:** English (United States)
* **Architecture:** x64
* **Edition:** Windows Server 2022
* **Installation type:** Desktop Experience

## Installation Process

After successfully booting the virtual machine from the mounted ISO, the Windows Server Setup environment was displayed.

### Operating System Edition

The following edition was selected:

**Windows Server 2022 Standard (Desktop Experience)**

The Desktop Experience option was selected to provide a graphical user interface for the laboratory environment.

This will make administration and configuration easier during the initial stages of the lab.

### Installation Type

The installation type selected was:

**Custom: Install Microsoft Server Operating System only (advanced)**

This option was selected because the virtual machine was created specifically for this Windows Server installation and did not contain an existing operating system that needed to be preserved.

### Virtual Disk

The Windows Server installation was performed on the unallocated space of the virtual disk assigned to the virtual machine.

Windows Setup automatically created the required system partitions.

### Installation

The installation process was started after selecting the target virtual disk.

Windows Server completed the installation and restarted the virtual machine.

After the installation completed, Windows Server prompted for the password for the built-in **Administrator** account.

A strong administrator password was configured.

> The administrator password is not stored in this repository or included in screenshots.

## First Login

After completing the installation, the server reached the Windows Server desktop successfully.

This confirms that:

* The Windows Server installation completed successfully.
* The virtual disk was detected correctly.
* The operating system booted successfully from the virtual disk.
* The graphical Desktop Experience environment is available.
* The Administrator account was configured successfully.

### Evidence

The first successful Windows Server desktop session is documented in:

```text
../screenshots/01-first-server-desktop.png
```

## Installation Result

The Windows Server 2022 virtual machine is now operational and ready for initial server configuration.

The next phase will cover the initial configuration of the Windows Server environment before implementing additional laboratory services and security controls.