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
- Group Policy Management Console
- Command Prompt

<h2>👨‍💻 Operating Systems Used </h2>

- Windows 11 Pro
- Windows Server 2022
- Windows 10 (21H2)

<h2>🧰 Implementation Steps</h2>

**Assuming you have completed the previous tutorial (<a href="https://github.com/JoshuaSchlegel/configure-ad">Active Directory Deployed in the Cloud (Azure)</a>), we will first need to get our VMs in the Azure Portal turned back on if they are off, log into our Domain Controller (DC1 VM), and open up "Group Policy".**

- Start the the VMs in Azure if needed
- Log in to DC1 as mydomain.com\jane_admin
- Pull up "Group Policy Management Console" by searching "Group Policy" in the search box in the taskbar
- Click the arrow next to "Forest: mydomain.com" -> "Domains" -> "mydomain.com"
- You'll see "Default Domain Policy" which you can right click and select edit **(We will use this existing policy to create the Password Lockout Policy. Later you will see how to create a new one for the Company Wallpaper Policy.)**
- After right clicking and selecting "edit" on the Default Domain Policy, Click the drop down arrows next to "Computer Configuration" -> "Policies" -> "Windows Settings" -> Click on "Security Settings" -> Double click on "Account Policies"
- Double click on "Account Lockout Policy"
- Double click on "Account lockout duration"
- Check the box next to "Define this policy setting" and set the time for 30 mins (Or however long you wish). Then click "Apply" and "OK"



![image](https://github.com/user-attachments/assets/28668c5e-79d4-4234-a8ab-4fd63f697b83)

![image](https://github.com/user-attachments/assets/eeeab035-0c87-4790-b51e-17d3616760b2)

![image](https://github.com/user-attachments/assets/870fbe89-b26d-4f82-81ad-7f8c357a86e1)

![image](https://github.com/user-attachments/assets/fded6875-c3be-44f3-9b3b-b6f8708817ac)

![image](https://github.com/user-attachments/assets/6e6da7cb-8f65-4978-b28f-487dcac3f59a)

![image](https://github.com/user-attachments/assets/4c1f96fe-1f4d-4cd7-9ca7-e0dacf49d736)

**Notice the other policies update as well. You will see that the Account lockout threshold is set to 5 inlaid logon attempts. Let us put that to the test 🙃**

- Back in GPM (Group Policy Management), right click on "Default Domain Policy" to rename it to "Account Lockout Policy"
- Then, right click on the "_EMPLOYEES" organizational unit and select "Link an Existing GPO..."
- Double click on "Account Lockout Policy" or click "OK"
- Log into Client1 as mydomain.com\jane_admin
- Pull up Command line as an admin
- Type "gpupdate /force" and hit Enter
- Type "gpresult /r"  and hit Enter to see where it shows "Account Lockout Policy" has been applied

![image](https://github.com/user-attachments/assets/cbc19b54-27c0-4bfb-a8c6-5eb44e76b41b)

![image](https://github.com/user-attachments/assets/924cecbb-7149-455f-9b66-70816b780821)

![image](https://github.com/user-attachments/assets/5855ebc4-c875-4381-99c9-bd4ab2155640)

![image](https://github.com/user-attachments/assets/ac8d948d-4a6e-4a3e-bc8b-8c585e933262)

![image](https://github.com/user-attachments/assets/30453ad3-57e0-475c-84d9-f8d74ee0cbc9)

- Open up "Active Directory Users and Computers" from the start menu
- Click on "_EMPLOYEES"
- Pick any user account that we created previously **(I suggest picking one that is easy/simple to remember.)**
- Log off of Client1 and get ready to log back into it as the user account you chose from _EMPLOYEES
- Use the "mydomain.com\user" username but instead of actually logging into Client1, get the password incorrect at least 5 times on purpose
- 
