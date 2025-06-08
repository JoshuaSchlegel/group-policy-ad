<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

![image](https://github.com/user-attachments/assets/0d08fdec-00b1-4b18-a9fe-1e0aa6d5b572)


<h1>Applying Group Policies in Active Directory Using Azure VM</h1>
This tutorial picks up directly where the <a href="https://github.com/JoshuaSchlegel/configure-ad">previous one</a> left off and outlines the use and implementation of group policies in Active Directory. Specifically password lockout policies 🔐 and a company wallpaper policy 🖼️. 

<h2>💻 Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Group Policy
- PowerShell

<h2>👨‍💻 Operating Systems Used </h2>

- Windows 11 Pro
- Windows Server 2022
- Windows 10 (21H2)

<h2>🧰 Implementation Steps</h2>

**Assuming you have completed the previous tutorial (<a href="https://github.com/JoshuaSchlegel/configure-ad">Active Directory Deployed in the Cloud (Azure)</a>), we will first need to get our VMs in the Azure Portal turned back on if they are off, log into our Domain Controller (DC1 VM), and open up "Group Policy".**

- Log in to DC1 as mydomain.com\jane_admin
- 






