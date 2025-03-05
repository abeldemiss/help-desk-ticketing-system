# Virtualization Setup Guide

This guide walks you through setting up a virtual lab environment using **VMware Workstation Pro** (free for personal use).
---

## Step 1: Install Virtualization Software

### VMware Workstation Pro (Recommended)
1. Download **VMware Workstation Pro** from the [official website](https://www.vmware.com/products/workstation-pro.html).
2. Install the software with default settings.
3. Verify that **hardware virtualization** is enabled in your BIOS/UEFI settings:
   - Restart your computer and enter BIOS/UEFI (usually by pressing `F2`, `F10`, `Del`, or `Esc` during boot).
   - Look for an option like **Intel VT-x** or **AMD-V** and enable it.
   - Save changes and exit.

---

## Step 2: Create a Virtual Machine (VM)

1. Open VMware Workstation Pro.
2. Click **Create a New Virtual Machine**.
3. Follow the wizard:
   - Select the **ISO file** for your operating system (e.g., Windows 10, Windows Server).
   - Allocate resources:
     - **RAM**: 4GB (minimum), 8GB (recommended).
     - **Storage**: 50GB (minimum), 100GB (recommended).
   - Choose **NAT** for network settings (default).
4. Complete the wizard and start the VM.

---

## System Requirements

### Minimum Requirements
- **RAM**: 8GB
- **Processor**: Dual-core with virtualization support (Intel VT-x/AMD-V).
- **Storage**: 100GB free space.
- **Host OS**: Windows 10/11, macOS, or Linux.

### Recommended Requirements
- **RAM**: 16GB
- **Processor**: Quad-core or higher.
- **Storage**: 200GB+ free space (for multiple VMs).
- **Host OS**: Windows 10 Pro or Enterprise.

---

## Screenshots
![VMware Installation](/Lab-Setup/Screenshots/vmware-install.png)
![VM Creation Wizard](/Lab-Setup/Screenshots/vm-creation.png)
![Windows 10 Installation](/Lab-Setup/Screenshots/windows10-install.png)