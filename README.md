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

***You should see a pop up message letting you know that the user account has been locked due to too many logon attempts and this should make us happy since we know it worked!***

  ![image](https://github.com/user-attachments/assets/e68f2a9d-c4f1-41ca-bfdc-2e05312715c1)

***Now lets be a good Sys Admin and unlock this user's account.*** 😏

- Back in DC1, pull up ADUC (Active Directory Users and Computers), and find the user that was locked out in "_EMPLOYEES"
- Right click on the user -> select "Properties", go to the "Account" tab and check the box next to "Unlock account"
- Click "Apply" and "OK"

![image](https://github.com/user-attachments/assets/c5d3f441-3b47-4aa5-b0b6-eead1cb33b47)

***Feel free to explore the other tabs within a user's properties to see what kind of info we can edit as well as controls we can apply. If you right click on the user, notice that it provides the option to disable the account as well. You can also create another Organizational Unit named "Assistants" or "Right Hands" just to provide those users with more access for certain tasks and what not. It's a great way to play around and practice managing users within Active Directory.*** 👍

***Now we can deploy a cool Company Wallpaper Policy for our users.***

- First, pull up "File Explorer" within DC1 from the start menu or by searching for it in the taskbar
- Navigate to the "Windows (C:)" drive and right click in the white space -> New -> Folder
- Name the folder "CompanyWallpaper"

![image](https://github.com/user-attachments/assets/243692b3-77a1-4b77-8cce-ce88969f1ede)


- Right click on the folder -> select "Properties"
- Click on the "Sharing" tab up top -> Click on "Share"
- Click the drop down and select "Find People"
- Type "domain users" in the text box and click on Check Names on right side -> Click "OK"
- Click "Share" on bottom right
***This is also how you would share folders/files within Active Directory to a Client's users and set permissions for those files such as "Read or Read/Write".***

![image](https://github.com/user-attachments/assets/1e594a9a-4839-41a6-aa0c-57ad3c5a2053)

![image](https://github.com/user-attachments/assets/b108e833-3911-41c1-9b9f-0df12271310b)

📝**NOTE:**
- ***Take note of the absolute path (UNC path) to this file as we will need it for the policy.***

![image](https://github.com/user-attachments/assets/8a35ef8a-4cce-44fa-addf-010f6b9e422a)

- Next, google a wallpaper you'd like to set for Client1 users within DC1 
- When you find it, make sure to right click and "save image as" so you can navigate to the "CompanyWallpaper" folder you created and save it in there and name it as "companywallpaper.jpg"

![image](https://github.com/user-attachments/assets/ec815f7b-65b4-4589-b6cc-922acca24276)

- The same way we started with the Account Lockout Policy, we will start with the Wallpaper Policy. Pull up the Group Policy Management Console within DC1
- Right click on "_EMPLOYEES" in Group Policy Management and select "Create a GPO in this domain, and Link it here.."
- Name it "Company Wallpaper Policy"
- Right click on the new GPO and select "Edit"
- You'll click on the drop down for "User Configuration" -> "Policies" -> "Administrative Templates" -> Select "Desktop"
- Double click "Desktop Wallpaper" in the right side (Notice how it gives you a Description of what the policy setting does after clicking on it once)

![image](https://github.com/user-attachments/assets/71f917c3-a7f6-4c46-9970-03b9e32bb0ec)

![image](https://github.com/user-attachments/assets/f2610f01-00b3-445a-9a2d-852970519190)

- Click the bullet next to "Enabled"
- Type the UNC or Absolute Path as the wallpaper name. If you've named everything as I have, it will be **\\DC1.mydomain.com\CompanyWallpaper\companywallpaper.jpg**
- Click "Apply" -> "OK"

![image](https://github.com/user-attachments/assets/e67eb65f-1312-4b27-97bc-80a1c0f60f7f)

***You should see the Company Wallpaper Policy under _EMPLOYEES in Group Policy Management now.***

![image](https://github.com/user-attachments/assets/95d5d398-9b1e-4a13-8757-6ea0d65bfeaa)

- Log into Client1 VM using Remote Desktop as mydomain.com\jane_admin
- Pull up Command Prompt as Admin to type the "gpupdate /force" command just like we did earlier with the Account Lockout Policy

![image](https://github.com/user-attachments/assets/563f5b4a-4256-469d-ab79-089d67174659)

- Log out of Client1 and log back in as the same user you used earlier or a different user if you'd like and be proud of Client1's new wallpaper

![image](https://github.com/user-attachments/assets/48a9f6ff-2807-4ba3-95d5-f388a5dce6e7)

 ***And that concludes this tutorial! I hope this helped and don't forget to delete your Resources from Microsoft Azure when you're done. Feel free to play around in Active Directory to practice and gain valuable experience! Cheers!***
