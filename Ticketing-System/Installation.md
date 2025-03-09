# Ticketing System Installation Guide

## Prerequisites
- Windows Server 2022 VM with Active Directory configured
- Administrator access
- Internet connectivity
- At least 4GB RAM
- 50GB free disk space

## Step 1: Install Prerequisites

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

![IIS Role Selection](../Screenshots/ticketing/iis-role.png)

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

![IIS Default Page](../Screenshots/ticketing/iis-default.png)

## Additional Resources

- [IIS Official Documentation](https://docs.microsoft.com/en-us/iis)
- [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
- [osTicket Requirements](https://docs.osticket.com/en/latest/Getting%20Started/Installation.html)
