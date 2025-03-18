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

![IIS Role Selection](/Ticketing-System/Screenshots/Installation/IIS.png)
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

![IIS Default Page](/Ticketing-System/Screenshots/Installation/IIS-Test.png)
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

# Configuring IIS to Use PHP on Windows Server 2022
---

## Step 1: Install CGI Feature
1. Open **Server Manager**.
2. Click **Manage** > **Add Roles and Features**.
3. Select **Role-based or feature-based installation** and click **Next**.
4. Select your **server** and click **Next**.
5. Under **Server Roles**, expand **Web Server (IIS)** > **Application Development**.
6. Check **CGI** and click **Next** > **Install**

---

## Step 2: Download and Install PHP
1. Go to [PHP for Windows](https://windows.php.net/download/).
2. Download the latest **Non-Thread Safe (NTS) x64** ZIP package.
3. Extract the ZIP file to `C:\php`.
4. Rename `php.ini-development` to `php.ini`.

---

## Step 3: Configure `php.ini`
1. Open `C:\php\php.ini` in Notepad.
2. Find and modify the following lines:

   ```ini
   extension_dir = "C:\php\ext"
   cgi.fix_pathinfo=1
   ```

3. Enable required extensions by removing `;` before these lines:
   ```ini
   extension=curl
   extension=mbstring
   extension=exif
   extension=mysqli
   extension=openssl
   ```

4. Save the file.

---

## Step 4: Add PHP to System Environment Variables
1. Open **System Properties** (`sysdm.cpl` in Run).
2. Go to **Advanced** > **Environment Variables**.
3. Under **System variables**, find **Path**, click **Edit**.
4. Click **New**, then enter:
   ```
   C:\php
   ```
5. Click **OK** and close.

![IIS Env](/Ticketing-System/Screenshots/Installation/php.png)
---

## Step 5: Configure IIS to Use PHP
1. Open **IIS Manager** (`inetmgr` in Run).
2. Select your **server** in the left panel.
3. Double-click **Handler Mappings**.
4. Click **Add Module Mapping...** and enter:
   - **Request Path:** `*.php`
   - **Module:** `FastCgiModule`
   - **Executable:** `C:\php\php-cgi.exe`
   - **Name:** `PHP via FastCGI`
5. Click **OK** and confirm.

![Module](/Ticketing-System/Screenshots/Installation/php-iis.png)
---

## Step 6: Restart IIS
- Open **Command Prompt (Admin)** and run:
  ```sh
  iisreset
  ```

---

## Step 7: Test PHP in IIS
1. Navigate to `C:\inetpub\wwwroot\`.
2. Create a new file `info.php` and add:
   ```php
   <?php phpinfo(); ?>
   ```
3. Open a browser and go to:
   ```
   http://localhost/info.php
   ```
4. If PHP is configured correctly, you should see the PHP info page.

---

## Install and Configure MySQL

### Step 1: Install MySQL

1. Download MySQL:
   - Go to [MySQL Downloads](https://dev.mysql.com/downloads/installer/)
   - Download MySQL Installer for Windows

2. Run MySQL Installer:
   - Launch the downloaded installer
   - Select `Custom` setup type
   - Click `Next`

3. Select Products:
   - Add `MySQL Server` (latest version)
   - Add `MySQL Workbench`
   - Click `Next`

![MySQL Installation](/Ticketing-System/Screenshots/Installation/mysql.png)
> Screenshot: MySQL Installer setup showing selected components

4. Configure MySQL Server:
   - Click `Next` through installation requirements
   - At Type and Networking, keep defaults
   - At Authentication Method, select `Use Legacy Authentication Method`
   - Set root password:
     ```
     Username: root
     Password: P@ssw0rd123
     ```
   - Note: Use a strong password in production
   - Create any additional user accounts if needed
   - Click `Next` through remaining steps
   - Click `Execute` to install

### Step 2: Create osTicket Database

1. Launch MySQL Workbench:
   - Start MySQL Workbench from Start menu
   - Connect to your local MySQL server
   - Enter root password when prompted

2. Create Database:
   - Click `Create new schema` button
   - Enter database details:
     ```
     Schema Name: osticket
     Character Set: utf8
     Collation: utf8_general_ci
     ```
   - Click `Apply`
   - Click `Apply` again in the confirmation dialog
   - Click `Finish`

![MySQL Database](/Ticketing-System/Screenshots/Installation/db.png)
> Screenshot: MySQL Workbench showing the osticket database creation

### Common Issues and Troubleshooting

1. MySQL Installation Fails:
   - Verify all prerequisites are installed
   - Check Windows Event Viewer for errors
   - Ensure no other MySQL instances are running

2. Cannot Connect to MySQL:
   - Verify MySQL service is running:
     ```powershell
     Get-Service MySQL80
     ```
   - Check port 3306 is not blocked
   - Verify credentials are correct

3. Database Creation Issues:
   - Ensure proper permissions for root user
   - Check available disk space
   - Verify character set support.
---

