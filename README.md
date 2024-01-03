<p align="center">
<img src="https://i.imgur.com/2o7JoFz.jpg" alt="File Sharing"/>
</p>

<h1>Network File Sharing in the Cloud (Azure)</h1>
This tutorial outlines the implementation of using Active Directory within Azure Virtual Machines to share files over the network and configuring permissions.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro

<h2>Prerequisite</h2>

- How to setup active directory in azure VM found here: https://github.com/jasonmolinet/configure-ad 

<h2>Deployment and Configuration Steps</h2>

Step 1:

![image](https://i.imgur.com/XVXUYdD.jpg)

Creating Folders
- Login as jane_admin or the adminstrative account created in DC-1 (Domain Controller)
- In DC-1 VM go to file explorer and create 4 folders in the C: labeled as “read-access”, “write-access”, “no-access”, “accounting”
- Then right click and properties on read access and share to domain users as permission level of read 
- Then right click and properties on write access and share to domain users as permission level of read/write
- Then right click and properties on no access and share to domain admins as permission level of read/write

Step 2:

![image](https://i.imgur.com/sPkt9bb.jpg)

Testing Permissions
- In DC-1, navigate to "Active Directory Users and Computers" in the search bar then _EMPLOYEES folder and copy down a random user to log into client-1 VM
- Go to client-1 VM and go to file explorer and look up \\DC-1
- In this folder, you can play around with each folder and try to access /write/read to see where the domain users have permission and where they don't
- Then go back to DC-1 VM and create a file in read access file as you are an admin (jane_admin)
- Then go back to client-1 VM and you can open the read access file that was created but can't create or edit something in that folder due to domain user permissions

Step 3:

![image](https://i.imgur.com/Z9IlWbQ.jpg)

Configuring Groups
- Now go back to DC-1 VM and go to active directory users and computers and create a new organization unit called _SECURITY_GROUPS
- In that folder right click and create a new group labeled "ACCOUNTANTS"
- In file explorer, right click on accounting folder and share this folder to the ACCOUNTANTS group and permission level of read and write
- Going back to client-1 VM, double clicking on the accounting folder in \\DC-1 you wont have permission as a domain user
- Go back to DC-1 VM and click on ACCOUNTANTS and click on members and add the user being used in client-1 VM
- Sometimes you got to sign out and sign back in so permissions are updated
- Logging back in with the user selected in DC-1 and has the security group permission then go to \\DC-1 and now you can open the accounting folder and create and edit files
