# Guide to Set Up osTicket

## Overview
This guide will walk you through the installation and configuration of osTicket, a widely-used open source help desk ticketing system. The installation process involves setting up prerequisites (IIS, PHP, and MySQL) and then installing osTicket itself.....

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

2. Create Database using SQL:
   - Click the SQL editor tab
   - Execute the following command to create the database:
     ```sql
     CREATE DATABASE osticket;
     ```

3. Create a dedicated database user:
   - In the same SQL editor, execute:
     ```sql
     CREATE USER 'osticketuser'@'localhost' IDENTIFIED BY 'yourpassword';
     GRANT ALL PRIVILEGES ON osticket.* TO 'osticketuser'@'localhost';
     FLUSH PRIVILEGES;
     ```
   - Replace `yourpassword` with a strong password
   - Note: This creates a dedicated user for osTicket with restricted permissions

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

## Step 3: Install osTicket

### Download and Extract osTicket

1. Download the latest version of osTicket:
   - Go to [osTicket Downloads](https://osticket.com/download/)
   - Download the latest version (the .zip file)

2. Extract the osTicket files:
   - Create a new folder named `osTicket` inside `C:\inetpub\wwwroot\`
   - Extract the contents of the downloaded ZIP file into this folder

### Access the Installation Page

1. Open your web browser and navigate to:
   ```
   http://localhost/osTicket/setup
   ```
2. The osTicket installation page should appear with a list of pre-installation checks

![osTicket Setup](/Ticketing-System/Screenshots/Installation/os-install.png)
> Screenshot: osTicket installation initial page

### Troubleshooting Initial Setup Issues

1. If you see a "Forbidden" error or no page load:
   - Ensure the wwwroot folder has the correct permissions
   - Verify that the IIS_IUSRS group has read and execute permissions
   - Check directory structure is correct (C:\inetpub\wwwroot\osTicket)

2. If the page loads but shows PHP extension errors:
   - Verify PHP extensions in php.ini (see Step 2 of PHP configuration)
   - Ensure all required extensions are enabled:
     ```ini
     extension=curl
     extension=mbstring
     extension=exif
     extension=mysqli
     extension=openssl
     ```
   - Restart IIS after making changes: `iisreset`

### Configure Database Connection

1. On the installation page, provide database configuration:
   - Database Name: `osticket`
   - Database Username: `osticketuser`
   - Database Password: `yourpassword` (the password you set when creating the user)
   - Database Host: `localhost`
   - Click `Install Now`

2. Manual Configuration (Alternative Method):
   - Navigate to the osTicket directory
   - Open the file `include/ost-config.php` in a text editor
   - Update the database settings with your credentials:
     ```php
     // Database Configuration
     define('DBHOST', 'localhost');         // Database Host
     define('DBNAME', 'osticket');          // Database Name
     define('DBUSER', 'osticketuser');      // Database Username
     define('DBPASS', 'yourpassword');      // Database Password
     ```
   - Save the file

### Troubleshooting Database Connection Issues

1. Error: "Cannot connect to database" or "Database not found":
   - Double-check database credentials (username, password, database name)
   - Ensure MySQL service is running:
     ```powershell
     Get-Service MySQL80
     ```
   - Check firewall settings for port 3306
   - Verify MySQL is accepting connections from localhost

### Configure Administrator Settings

1. Provide admin information:
   - Admin Email: (your email address)
   - Admin Name: (name for admin user)
   - Default Email: (system email for notifications)

2. If email configuration fails:
   - Verify SMTP settings
   - Check correct port configuration (25, 587, etc.)
   - Ensure firewall isn't blocking SMTP traffic

### Finalize Installation

1. Complete the installation process:
   - Click through remaining prompts
   - When installation completes, you'll see a success message

2. If you encounter errors during final installation:
   - Check file permissions on osTicket folder
   - Verify IIS user has write access to the configuration directory
   - Review PHP error logs in Event Viewer

   ![Complete](/Ticketing-System/Screenshots/Installation/complete.png)

### Post-Installation Security

1. Remove the setup directory:
   - Delete the `setup` folder from `C:\inetpub\wwwroot\osTicket\`
   - If unable to delete:
     ```powershell
     iisreset /stop
     # Delete the folder
     iisreset /start
     ```

2. Secure the configuration file:
   - Set read-only permissions on `C:\inetpub\wwwroot\osTicket\include\ost-config.php`

### Verify Installation

1. Access the staff panel:
   ```
   http://localhost/osTicket/scp
   ```
2. Log in with admin credentials
3. Create a test ticket to verify functionality

![Admin Login](/Ticketing-System/Screenshots/Installation/admin.png)
> Screenshot: osTicket admin login page

### Common Installation Issues

1. "Missing Extensions" Errors:
   - Check php.ini for required extensions
   - Restart IIS after modifying php.ini: `iisreset`

2. File Permission Issues:
   - Ensure IIS_IUSRS group has appropriate permissions:
     ```powershell
     icacls "C:\inetpub\wwwroot\osTicket" /grant "IIS_IUSRS:(OI)(CI)(M)"
     ```

3. Email Sending Issues:
   - Verify SMTP configuration
   - Check for network/firewall restrictions
   - Test with a simplified mail configuration first


## Additional Resources

- [osTicket Documentation](https://docs.osticket.com/)
- [osTicket Forum](https://forum.osticket.com/)
- [PHP Documentation](https://www.php.net/docs.php)
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [IIS Documentation](https://docs.microsoft.com/en-us/iis/)

