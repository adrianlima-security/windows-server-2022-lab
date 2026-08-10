# Windows Server 2022 Lab

## 01 — Virtual Machine Creation

### Overview

This document describes the creation and initial configuration of the Windows Server 2022 virtual machine used in the cybersecurity home lab.

The virtual machine was created using VMware Workstation Pro and will provide a Windows Server environment for security, networking, identity, monitoring, and detection engineering exercises.

### Virtualization Platform

* **Hypervisor:** VMware Workstation Pro
* **Virtual machine:** Windows Server 2022 Lab
* **Operating system:** Windows Server 2022
* **Architecture:** x64

### Installation Media

The Windows Server installation was performed using the Microsoft Windows Server 2022 Evaluation ISO.

**ISO file:**

```text
SERVER_EVAL_X64FRE_en-us.iso
```

The ISO is the English (United States) x64 evaluation image.

### Virtual Machine Configuration

The virtual machine was configured with the following resources:

| Resource        | Configuration                      |
| --------------- | ---------------------------------- |
| CPU             | 2 processors × 2 cores             |
| Total vCPU      | 4                                  |
| RAM             | 4 GB                               |
| Storage         | 80 GB virtual disk                 |
| Network Adapter | NAT                                |
| CD/DVD          | Windows Server 2022 Evaluation ISO |

The virtual hardware configuration provides a lightweight Windows Server environment suitable for the initial stages of the cybersecurity home lab.

### Network Configuration

The virtual machine uses a **NAT** network adapter.

NAT allows the Windows Server virtual machine to access external network resources through the host machine while keeping the virtual machine behind VMware's network address translation.

This configuration is appropriate for the initial laboratory setup because the server does not need to be directly exposed to the physical network.

> **Note:** Hardware resource values will be documented based on the final VMware configuration to ensure the documentation accurately reflects the lab environment.

### ISO Configuration

The Windows Server 2022 ISO was mounted through the virtual CD/DVD device in VMware Workstation Pro.

The following configuration was used:

* **CD/DVD device:** Virtual CD/DVD drive
* **Installation media:** Windows Server 2022 Evaluation ISO
* **Connect at power on:** Enabled
* **ISO:** `SERVER_EVAL_X64FRE_en-us.iso`

### Boot Troubleshooting

During the initial boot attempt, the virtual machine displayed:

```text
EFI Network...
```

This indicated that the VM was continuing to another boot option instead of immediately starting the Windows Server installation.

After restarting the VM, the following prompt appeared:

```text
Press any key to boot from CD or DVD...
```

The VMware console initially did not register the keyboard input correctly when using the `Enter` key.

The issue was resolved by clicking inside the virtual machine console to ensure keyboard focus and pressing the **Space** key when the boot prompt appeared.

The Windows Server installation then started successfully.

### Result

The virtual machine successfully booted from the Windows Server 2022 installation media.

The Windows Server setup process was then started.

### Evidence

Screenshots related to the VM creation and boot process are stored in:

```text
screenshots/
```

Relevant screenshots will be referenced from this document as the laboratory documentation progresses.