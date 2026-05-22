
sTicket Installation & Configuration Lab Guide
Complete hands-on lab for installing and configuring osTicket v1.15.8 on Windows 10 with Azure VM.

📋 Table of Contents

Part 1: Azure VM Setup
Part 2: Download Installation Files
Part 3: Install IIS with CGI
Part 4: Install PHP Manager & Dependencies
Part 5: PHP Installation & Configuration
Part 6: MySQL Database Setup
Part 7: osTicket Installation
Part 8: osTicket Web Configuration
Part 9: Post-Installation Cleanup
Part 10: Configuration - Roles & Departments
Part 11: Configuration - Agents & Users
Part 12: Configuration - SLAs & Help Topics
Part 13: Lab Exercises - Ticket Management


Part 1: Azure VM Setup
Step 1.1: Create Azure Virtual Machine
VM Specifications:

Operating System: Windows 10 Pro
Name: osticket-vm
vCPUs: 4
RAM: 8 GB recommended
Storage: 30 GB

Login Credentials:
Username: labuser
Password: osTicketPassword1!
⚠️ IMPORTANT SECURITY NOTE:
Never store passwords in plain text in production documentation. Use a password manager:

KeePass - Open source, self-hosted
LastPass - Cloud-based
NordPass - Commercial solution

Step 1.2: Connect to VM via Remote Desktop

Open Remote Desktop Connection on your local machine
Enter VM's IP address or hostname
Use credentials:

Username: labuser
Password: osTicketPassword1!


Accept certificate warnings and connect


Part 2: Download Installation Files
Step 2.1: Download osTicket Installation Package

Within the VM, open Microsoft Edge or Firefox
Navigate to: osTicket-Installation-Files.zip
Download the file to Desktop
Extract the ZIP file
Rename folder to: osTicket-Installation-Files

Step 2.2: Verify Downloaded Files
After extraction, you should have:
osTicket-Installation-Files/
├── php-7.3.8-nts-Win32-VC15-x86.zip
├── PHPManagerForIIS_V1.5.0.msi
├── rewrite_amd64_en-US.msi
├── VC_redist.x86.exe
├── mysql-5.5.62-win32.msi
├── osTicket-v1.15.8.zip
├── HeidiSQL setup file
└── [other dependencies]

Part 3: Install IIS with CGI
Step 3.1: Enable IIS on Windows 10

Open Control Panel
Navigate to: Programs → Programs and Features
Click: Turn Windows features on or off
Locate: Internet Information Services (IIS)
Check the box to enable IIS

Step 3.2: Enable CGI and Application Development

Expand Internet Information Services
Expand World Wide Web Services
Expand Application Development Features
Enable (check): CGI
Click OK to install

Step 3.3: Verify IIS Installation

Open Internet Explorer or Microsoft Edge
Navigate to: http://localhost
You should see the IIS welcome page
If successful, IIS is installed and running


Part 4: Install PHP Manager & Dependencies
Step 4.1: Install PHP Manager for IIS

Navigate to: osTicket-Installation-Files folder on Desktop
Double-click: PHPManagerForIIS_V1.5.0.msi
Follow the installation wizard
Accept default settings
Click Finish

Step 4.2: Install IIS Rewrite Module

In osTicket-Installation-Files folder
Double-click: rewrite_amd64_en-US.msi
Accept license agreement
Click Install
Click Finish

Step 4.3: Install Visual C++ Redistributable

In osTicket-Installation-Files folder
Double-click: VC_redist.x86.exe
Accept license terms
Click Install
Click Finish (restart may be required)


Part 5: PHP Installation & Configuration
Step 5.1: Create PHP Directory

Open File Explorer
Navigate to: *C:* (Local Disk C)
Right-click in empty space → New → Folder
Name the folder: PHP
You now have: C:\PHP

Step 5.2: Extract PHP to C:\PHP

Navigate to: osTicket-Installation-Files on Desktop
Right-click: php-7.3.8-nts-Win32-VC15-x86.zip
Select: Extract All...
Extract to: C:\PHP
Click Extract

Step 5.3: Configure PHP in IIS

Open Internet Information Services (IIS) Manager

Press: Win + R
Type: inetmgr
Press: Enter


In IIS Manager left panel:

Click on your computer name
Double-click: PHP Manager


In the right panel:

Click: Register new PHP version


Browse to PHP executable:

Navigate to: C:\PHP\php-cgi.exe
Click OK


Verify PHP is registered:

Check for PHP version (7.3.8)
Should show "PHP is installed and configured properly"



Step 5.4: Reload IIS

In IIS Manager, right-click your computer name
Select: Restart or
Click Stop then Start the server
Verify status shows "Started"


Part 6: MySQL Database Setup
Step 6.1: Install MySQL 5.5.62

Navigate to: osTicket-Installation-Files
Double-click: mysql-5.5.62-win32.msi
Click Next through the installer

Step 6.2: MySQL Configuration
During installation:

Select: Typical Setup → Next
After installation completes:

Launch Configuration Wizard (should auto-launch)
Select: Standard Configuration
Click Next


MySQL Server Instance Configuration:

Select: Install As Windows Service
Service Name: MySQL5
Click Next


Set MySQL Root Password:

Current password: (leave blank)
New root password: root
Confirm password: root
Click Next


Click Execute to apply configuration
Click Finish to complete setup

Step 6.3: Verify MySQL Installation

Open Command Prompt as Administrator
Test MySQL connection:

cmd   mysql -u root -p

When prompted for password, type: root
You should see the MySQL prompt: mysql>
Type: EXIT to close MySQL CLI


Part 7: osTicket Installation
Step 7.1: Extract osTicket Files

Navigate to: osTicket-Installation-Files on Desktop
Right-click: osTicket-v1.15.8.zip
Select: Extract All...
Extract to a temporary location on Desktop

Step 7.2: Copy osTicket to Web Root

In File Explorer, navigate to extracted osTicket folder
Locate the upload folder inside
Copy the upload folder
Navigate to: C:\inetpub\wwwroot
Paste the upload folder here

Step 7.3: Rename Upload Folder to osTicket

In C:\inetpub\wwwroot
Right-click the upload folder
Select: Rename
Type: osTicket
Press Enter

Step 7.4: Reload IIS

Open IIS Manager (Win + R → inetmgr)
Right-click your computer name
Click Restart the server
Wait for restart to complete


Part 8: osTicket Web Configuration
Step 8.1: Enable Required PHP Extensions

Open IIS Manager
Navigate: Sites → Default → osTicket
Double-click: PHP Manager
Click: Enable or disable an extension
Enable the following extensions:

 php_imap.dll - Click to enable
 php_intl.dll - Click to enable
 php_opcache.dll - Click to enable


Click OK

Step 8.2: Test osTicket Access

In IIS Manager:

Right-click: osTicket site
Select: *Browse :80 (or click the link on the right)


osTicket installer should load in browser
Observe which extensions are not enabled (red X marks)
Extensions should now show as enabled (green checkmarks)

Step 8.3: Configure ost-config.php

Navigate to: C:\inetpub\wwwroot\osTicket\include
Find: ost-sampleconfig.php
Right-click and select: Rename
Change name to: ost-config.php

Step 8.4: Set Permissions on ost-config.php

Right-click: ost-config.php
Select: Properties
Go to: Security tab
Click: Advanced
Click: Disable inheritance
Select: Remove all inherited permissions
Click Apply
Click: Edit (add new permission)
Click: Add
Type: Everyone
Click Check Names
Click OK
Select: Everyone
Check: Full Control (or at minimum: Modify, Read, Write)
Click Apply → OK
Close Properties


Part 9: osTicket Web Installation Wizard
Step 9.1: Start Installation

In browser, navigate to: http://localhost/osTicket
Click: Continue
Fill in Helpdesk Information:

Helpdesk Name: osTicket Helpdesk (or your choice)
Default Email: support@osticket.local
URL: http://localhost/osTicket/



Step 9.2: Create Database in HeidiSQL

Open HeidiSQL (download if not installed from Installation-Files)
Create a new session:

Hostname: localhost
User: root
Password: root
Click Open


In HeidiSQL:

Right-click: Databases
Select: Create New → Database
Name: osTicket
Collation: utf8_general_ci
Click OK



Step 9.3: Configure Database in Installation Wizard
Back in browser, continue osTicket installation:

Database Settings:

MySQL Hostname: localhost
MySQL Database: osTicket
MySQL Username: root
MySQL Password: root


Click: Install Now!
Wait for installation to complete
You should see: "Congratulations! osTicket is installed!"


Part 10: Post-Installation Cleanup
Step 10.1: Delete Setup Directory
⚠️ SECURITY CRITICAL: The setup directory is a security vulnerability and must be removed.

Navigate to: C:\inetpub\wwwroot\osTicket
Delete the setup folder entirely
Verify it's removed

Step 10.2: Change ost-config.php Permissions

Navigate to: C:\inetpub\wwwroot\osTicket\include
Right-click: ost-config.php
Select: Properties → Security → Advanced
Select: Everyone
Click: Edit
Uncheck all except:

☑ Read
☑ Read & Execute


Click Apply → OK

This ensures the file cannot be modified after installation.

Part 11: Configuration - Roles & Departments
Step 11.1: Access Admin Panel

Navigate to: http://localhost/osTicket/scp/login.php
Login with admin credentials created during installation
You are now in the Admin Panel

Step 11.2: Create Roles
Roles define permission levels for agents.

Navigate: Admin Panel → Agents → Roles
Click: Add New Role
Create role: Supreme Admin

Role Name: Supreme Admin
Permissions: Check All permissions
Click Add Role



Step 11.3: Create Departments
Departments organize tickets and agents by business unit.

Navigate: Admin Panel → Agents → Departments
Click: Add New Department
Create department: SysAdmins

Department Name: SysAdmins
Status: Active
Parent Department: Top Level Department (change if needed)
Click Create Dept


Create department: Support

Department Name: Support
Status: Active
Parent Department: Top Level Department
Click Create Dept


Create department: Online Banking

Department Name: Online Banking
Status: Active
Parent Department: Top Level Department
Click Create Dept



Lab Note: Later, you will delete the "Maintenance" department if it exists (do NOT archive, DELETE it).
Step 11.4: Create Teams
Teams allow pulling agents from different departments.

Navigate: Admin Panel → Agents → Teams
Click: Add New Team
Create team: Online Banking

Team Name: Online Banking
Team Lead: (will select after creating agents)
Click Create Team




Part 12: Configuration - Agents & Users
Step 12.1: Create Agents (Help Desk Staff)
Agents are help desk professionals who handle tickets.

Navigate: Admin Panel → Agents → Agents
Click: Add New Agent

Agent 1: Jane

First Name: Jane
Last Name: [Your choice]
Email: jane@osticket.local
Department: SysAdmins
Role: Supreme Admin
Status: Active
Password: Set password for Jane
Click Create


Click: Add New Agent again

Agent 2: John

First Name: John
Last Name: [Your choice]
Email: john@osticket.local
Department: Support
Role: Supreme Admin
Status: Active
Password: Set password for John
Click Create

Step 12.2: Create Users (Customers)
Users are end users who create tickets (not agents).

Navigate: Agent Panel → Users → Add New (or Users directory)

Note: Switching between Admin and Agent Panel:

Admin Panel: Full system configuration
Agent Panel: Ticket management and user management


Create user: Karen

Full Name: Karen
Email: karen@example.com
Click Add User


Create user: Ken

Full Name: Ken
Email: ken@example.com
Click Add User




Part 13: Configuration - SLAs & Help Topics
Step 13.1: Configure Service Level Agreements (SLAs)
SLAs define response and resolution timeframes.

Navigate: Admin Panel → Manage → SLA
Click: Add New SLA Plan

SLA 1: Sev-A

Name: Sev-A
Grace Period: 1 hour
Schedule: 24/7
Click Add Plan

SLA 2: Sev-B

Name: Sev-B
Grace Period: 4 hours
Schedule: 24/7
Click Add Plan

SLA 3: Sev-C

Name: Sev-C
Grace Period: 8 hours
Schedule: Business Hours (default)
Click Add Plan

Step 13.2: Configure Help Topics
Help Topics categorize tickets when users create them.

Navigate: Admin Panel → Manage → Help Topics
Click: Add New Help Topic

Create the following help topics:
1. Business Critical Outage

Topic Name: Business Critical Outage
Department: Online Banking
SLA Plan: Sev-A
Click Add Topic

2. Personal Computer Issues

Topic Name: Personal Computer Issues
Department: Support
SLA Plan: Sev-B
Click Add Topic

3. Equipment Request

Topic Name: Equipment Request
Department: Support
SLA Plan: Sev-C
Click Add Topic

4. Password Reset

Topic Name: Password Reset
Department: Support
SLA Plan: Sev-C
Click Add Topic

5. Other

Topic Name: Other
Department: Support
SLA Plan: Sev-C
Click Add Topic

Step 13.3: Configure User Settings
Allow anyone to create tickets (but require registration):

Navigate: Admin Panel → Settings → User Settings
Registration Required:

☑ Check: Require registration and login to create tickets


This allows unregistered users to self-register
Click Save Changes


Part 14: Lab Exercises - Ticket Management
🎯 Exercise 1: Banking System Outage Ticket
As End User (Karen):

Navigate to: http://localhost/osTicket/
Click: Open a New Ticket
Fill in ticket details:

Email: karen@example.com (or your registered email)
Full Name: Karen
Subject: entire mobile/online banking system is down
Help Topic: Business Critical Outage
Issue Summary: Describe the outage in detail
Click Create Ticket


Note the Ticket ID (you'll need this)

As Help Desk Agent (John):

Navigate to: http://localhost/osTicket/scp/login.php
Login as: John
Navigate: Tickets → Open
Find the ticket "entire mobile/online banking system is down"
Click to open the ticket
Observe the ticket properties:

☐ Priority: (what is it?)
☐ Department: (which department?)
☐ SLA Plan: (which SLA?)
☐ Assigned To: (who is it assigned to?)
☐ Status: (Open, Pending, etc.)



Set Ticket Properties:

Click: Edit on the ticket
Change properties:

Priority: Emergency (Sev-A)
Department: Online Banking
SLA Plan: Sev-A (1 hour, 24/7)
Click Save



Attempt to Access as John:

John is in Support department
Try to view or edit this ticket
Question: Can John view this ticket? Can he change properties?

Expected: John may NOT be able to view this ticket because it's assigned to Online Banking department



Complete Ticket as Jane (SysAdmins Department):

Logout as John
Login as: Jane (SysAdmins department)
Navigate: Tickets → Open
Find the Banking System ticket
Click to view and respond
Click: Reply or Post Note
Add response:

   We have identified the issue with the mobile banking platform.
   Our engineering team is working on restoration. Will update in 1 hour.

Click: Reply or Post
Click: Close Ticket to mark as resolved
Change Status to: Closed
Click Save


🎯 Exercise 2: Adobe Upgrade Ticket
As End User (Ken):

Navigate to: http://localhost/osTicket/
Click: Open a New Ticket
Fill in:

Email: ken@example.com
Full Name: Ken
Subject: accounting department needs adobe upgrade, broken
Help Topic: Personal Computer Issues
Details: Accounting software (Adobe Creative Suite) is not working and needs upgrading
Click Create Ticket



As Help Desk Agent (John):

Login as: John
Navigate: Tickets → Open
Find ticket: "accounting department needs adobe upgrade, broken"
Observe properties:

☐ Priority: (what is it currently?)
☐ Department: (where is it assigned?)
☐ SLA Plan: (which SLA?)
☐ Assigned To: (who?)



Set Ticket Properties:

Click: Edit
Change:

Priority: High/Urgent (Sev-B)
Department: Support
SLA Plan: Sev-B (4 hours, 24/7)
Click Save



Work the Ticket as John:

Click: Reply
Add response:

   Hi Ken,

   Thank you for reporting this issue. I understand Adobe Creative Suite 
   is not launching properly in your Accounting department.

   I will coordinate with our software team to schedule an upgrade. 
   Expected completion: within 4 hours.

   I'll follow up with you shortly.

   Best regards,
   John

Click: Post Reply
After response, click: Close Ticket (mark as resolved)
Set Status: Resolved
Click Save


🎯 Exercise 3: CFO's Laptop Issue
As End User (Karen):

Navigate to: http://localhost/osTicket/
Click: Open a New Ticket
Fill in:

Email: karen@example.com
Full Name: Karen
Subject: CFO's laptop will no longer turn on
Help Topic: Personal Computer Issues
Details: The CFO's laptop is completely unresponsive and won't power on. This is urgent as they have important meetings today.
Click Create Ticket



As Help Desk Agent (John):

Login as: John
Navigate: Tickets → Open
Find ticket: "CFO's laptop will no longer turn on"
Observe ticket properties:

☐ Priority
☐ Department
☐ SLA Plan
☐ Assigned To



Set Ticket Properties:

Click: Edit
Configure:

Priority: High (Sev-B)
Department: Support
SLA Plan: Sev-B (4 hours, 24/7)
Assigned To: John
Click Save



Work the Ticket as John:

Click: Reply
Add response:

   Hi Karen,

   I understand the CFO's laptop is not powering on. This is indeed urgent.

   I will immediately:
   1. Visit the CFO's office to assess the hardware
   2. Check power connections and battery status
   3. Perform basic troubleshooting
   4. If needed, arrange for hardware replacement from our spare inventory

   I'm heading there now and will have an update within 30 minutes.

   Thanks,
   John

Click: Post Reply
Continue with ticket resolution:

Add more replies as you "troubleshoot" and resolve the issue
Once resolved, click: Close Ticket
Set Status: Resolved
Click Save




🎓 Lab Completion Summary
What You've Accomplished:
✅ Created and configured an Azure VM
✅ Installed IIS with CGI support
✅ Installed PHP Manager and dependencies
✅ Configured PHP 7.3.8 in IIS
✅ Installed MySQL 5.5.62 database
✅ Installed osTicket v1.15.8
✅ Configured osTicket web interface
✅ Created Roles and Departments
✅ Created Agents and Users
✅ Configured SLAs and Help Topics
✅ Completed ticket management exercises
✅ Tested ticket workflows between agents and departments
Key Concepts Learned:
📚 Roles: Permission sets for agents (Supreme Admin, Agent, etc.)
📚 Departments: Business unit organization (SysAdmins, Support, etc.)
📚 Teams: Cross-departmental agent groups (Online Banking)
📚 Agents: Help desk staff who handle tickets
📚 Users: End users who create tickets
📚 SLAs: Service level agreements defining response timeframes
📚 Help Topics: Ticket categories for routing and SLA assignment
📚 Ticket Properties: Priority, Department, SLA, Assigned To, Status
📚 Ticket Workflow: Creation → Assignment → Resolution → Closure
Important URLs:

Admin Panel: http://localhost/osTicket/scp/login.php
Agent Panel: http://localhost/osTicket/scp/login.php (login as agent)
User Portal: http://localhost/osTicket/
IIS Manager: Win + R → inetmgr


🔐 Security Reminders
✅ Setup directory deleted (critical!)
✅ ost-config.php permissions set to Read-only (critical!)
✅ Database credentials documented securely (NOT in plain text in production)
✅ Regular backups scheduled (best practice)
✅ SSL/HTTPS enabled (in production)

📝 Troubleshooting
osTicket Page Shows Errors:
Solution: Ensure all PHP extensions are enabled:

 php_imap.dll
 php_intl.dll
 php_opcache.dll

Reload IIS after enabling extensions.
Cannot Connect to MySQL:
Solution: Verify MySQL is running:

Open Services (services.msc)
Look for MySQL5 service
Ensure Status shows "Running"

Cannot Login to osTicket:
Solution:

Verify ost-config.php permissions are correct
Check MySQL database exists (verify in HeidiSQL)
Restart IIS

File Permission Issues:
Solution: Re-check NTFS permissions on osTicket folder:

Right-click folder → Properties → Security
Ensure IIS AppPool identity has Modify permissions


📚 Additional Resources

osTicket Documentation: https://docs.osticket.com/
MySQL Documentation: https://dev.mysql.com/doc/
PHP Documentation: https://www.php.net/docs.php
IIS Documentation: https://docs.microsoft.com/en-us/iis/
