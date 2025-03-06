# Windows Server VM Setup Guide for VMware Workstation Pro

## Prerequisites

- VMware Workstation Pro 17.x installed
- Windows Server 2022/2019 ISO file downloaded
- Minimum host system requirements:
  - 16GB RAM (32GB recommended)
  - 4 CPU cores (8 recommended)
  - 100GB free disk space
  - CPU with virtualization support (Intel VT-x or AMD-V)

## Step-by-Step VM Creation

### 1. Initial VM Setup
1. Open VMware Workstation Pro
2. Click `File` → `New Virtual Machine`
3. Select `Custom (advanced)` → Click `Next`
4. Hardware compatibility: Choose `Workstation 17.x`
5. Select `I will install the operating system later` → Click `Next`
6. Guest OS:
   - Select `Microsoft Windows`
   - Version: `Windows Server 2022`
   - Click `Next`

### 2. VM Name and Location
1. Enter VM Name: `DC01` (or your preferred name)
2. Location: Choose your VM storage location
3. Click `Next`

### 3. Processor Configuration
1. Number of processors: `2`
2. Number of cores per processor: `1`
4. Click `Next`

### 4. Memory Allocation
1. Set Memory: `4096 MB` (4GB) minimum
2. Click `Next`

### 5. Network Configuration
1. Network Type: `Use network address translation (NAT)`
   - This allows internet access while keeping VM isolated
2. Click `Next`

### 6. Disk Configuration
1. Select `Create a new virtual disk`
2. Select `SCSI` as the disk type
3. Disk size:
   - Set to `100 GB`
   - Select `Store virtual disk as a single file`
4. Click `Next`
5. Keep default disk file name → Click `Next`
6. Click `Finish` to create the VM

## Windows Server Installation

### 1. Install Windows Server
1. Right-click the VM → `Settings`
2. Select `CD/DVD (SATA)`
3. Choose `Use ISO image file`
4. Browse to your Windows Server ISO file
5. Click `OK`
6. Power on the VM
7. Press any key to boot from DVD when prompted
8. Select language preferences → Click `Next`
9. Click `Install Now`

### 2. Operating System Selection
1. Choose:
   - For GUI: `Windows Server 2022 Standard (Desktop Experience)`
2. Accept license terms
3. Choose `Custom: Install Windows only`
4. Select the disk → Click `Next`

### 3. Initial Configuration
1. Set Administrator password when prompted
2. Log in with credentials
3. Wait for Server Manager to load

### 4. Post-Installation Tasks

#### Install VMware Tools
1. Click `VM` → `Install VMware Tools`
2. If AutoPlay doesn't start:
   - Open File Explorer
   - Double-click DVD drive
   - Run `setup64.exe`
3. Follow installation wizard
4. Restart when prompted

#### Configure Network Settings
1. Open Server Manager
2. Click `Local Server`
3. Click IPv4 address assignment
4. Right-click network adapter → `Properties`
5. Select `Internet Protocol Version 4`
6. Configure static IP:
   ```
   IP address: 192.168.1.10 (or your preferred IP)
   Subnet mask: 255.255.255.0
   Default gateway: 192.168.1.1
   Preferred DNS: 127.0.0.1 (after DC promotion)
   ```

#### Windows Updates
1. Open Settings
2. Click `Update & Security`
3. Click `Check for updates`
4. Install all available updates
5. Restart if required

## Performance Optimization

### 1. VM Settings Optimization
1. Right-click VM → `Settings`
2. Disable unnecessary devices:
   - Printer
   - Sound card
   - USB controller (if not needed)

### 2. Windows Server Optimization
1. Disable unnecessary services:
   - Windows Search
   - Print Spooler (if not needed)
2. Configure power settings to High Performance

## Next Steps

After completing this setup:
1. Proceed to `Active-Directory.md` for DC promotion
2. Configure Windows Firewall settings
3. Set up backup schedule
4. Document IP configuration

## Troubleshooting

### Common Issues

1. VM fails to start
   - Verify virtualization is enabled in BIOS
   - Check host system resources
   - Verify VMware services are running

2. Network connectivity issues
   - Verify VM network adapter settings
   - Check host network configuration
   - Ensure VMware network services are running

3. Performance issues
   - Monitor host resource usage
   - Adjust VM resource allocation
   - Check for Windows updates

### Support Resources
- [VMware Workstation Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/index.html)
- [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
- [Microsoft Support](https://support.microsoft.com/)
