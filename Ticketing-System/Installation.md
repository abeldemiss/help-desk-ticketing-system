# Guide to Set Up osTicket

## Overview
This guide will walk you through the installation and configuration of osTicket, a widely-used open source help desk ticketing system. The installation process involves setting up prerequisites (IIS, PHP, and MySQL) and then installing osTicket itself.

## Prerequisites
- Windows Server 2022 VM with Active Directory configured
- Administrator access
- Internet connectivity
- At least 4GB RAM
- 50GB free disk space
- Downloads needed:
  - PHP for Windows
  - MySQL Server
  - osTicket latest version

## Step 1: Install Prerequisites

osTicket requires three main components:
- Web server (IIS)
- PHP
- MySQL database

### 1.1 Install Internet Information Services (IIS)

1. Open Server Manager:
   - Launch Server Manager if not already open
   - Click `Dashboard` if not already selected

2. Add IIS Role:
   - Click `Add roles and features`
   - Click `Next` at the Before You Begin screen
   - Select `Role-based or feature-based installation`
   - Click `Next`
   - Select your server from the server pool
   - Click `Next`

3. Select IIS Role:
   - In the Roles list, find and select `Web Server (IIS)`
   - Click `Add Features` when prompted for required features
   - Click `Next`

![IIS Role Selection](/Ticketing-System/Screenshots/Installation/iis.png)
> Screenshot: Add Roles and Features Wizard with Web Server (IIS) selected

4. Select IIS Features:
   - Keep default features selected
   - Click `Next` through Features
   - At Web Server Role (IIS) Services, click `Next`
   - Review Role Services
   - Ensure these are selected:
     - Common HTTP Features (all)
     - Application Development Features:
       - .NET Extensibility
       - ASP.NET
       - CGI
       - ISAPI Extensions
       - ISAPI Filters
     - Security Features:
       - Basic Authentication
       - Windows Authentication
   - Click `Next`

5. Complete Installation:
   - Review selections
   - Click `Install`
   - Wait for installation to complete
   - Click `Close` when finished

6. Verify IIS Installation:
   - Open web browser
   - Navigate to `http://localhost`
   - You should see the default IIS welcome page

![IIS Default Page](/Ticketing-System/Screenshots/Installation/iis-test.png)
> Screenshot: IIS default welcome page showing successful installation

### Common Issues and Troubleshooting

1. IIS Installation Fails:
   - Check Windows Update is current
   - Verify sufficient disk space
   - Review Windows Event Viewer for specific errors

2. Cannot Access Default Page:
   - Verify World Wide Web Publishing Service is running:
     ```powershell
     Get-Service W3SVC | Select Status,Name,DisplayName
     ```
   - Check Windows Firewall:
     - Verify port 80 is open
     - Allow IIS through firewall

3. Permission Issues:
   - Verify IIS_IUSRS group permissions
   - Check Application Pool identity permissions

## Next Steps

After completing IIS installation:
1. Install PHP (Section 1.2)
2. Install MySQL (Section 1.3)
3. Configure IIS for PHP
4. Install osTicket

## Additional Resources

- [IIS Official Documentation](https://docs.microsoft.com/en-us/iis)
- [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
- [osTicket Requirements](https://docs.osticket.com/en/latest/Getting%20Started/Installation.html)
- [PHP for Windows](https://windows.php.net/download/)
- [MySQL Community Downloads](https://dev.mysql.com/downloads/installer/)
