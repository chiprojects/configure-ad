<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Full Video Demonstrations</h2>

- ### [YouTube: How to Configure Active Directory Infrastructure within Azure](https://youtu.be/LLMdGsnudck) - Part 1

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://youtu.be/9SDWDI7HGvo) - Part 2

- ### [YouTube: How to Create Users with Powershell](https://youtu.be/ozonZSqeWPM) - Part 3


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Part 1: Preparing the Active Directory Infrastructure in Azure
- Part 2: Installing and Deploying Active Directory
- Part 3: Creating Users with Powershell
- Part 4: Group Policy and Managing User Accounts

<h2>Deployment and Configuration Steps</h2>

<h3>Part 1: Preparing the Active Directory Infrastructure</h3>

[![Video Title](https://img.youtube.com/vi/LLMdGsnudck/0.jpg)](https://youtu.be/LLMdGsnudck?si=UQNqhanKu6ccev3g)

<b>1) Setup Domain Controller</b>

- Navigate to the Azure homepage to create a `Resource Group` and select `Review + Create` on the bottom left. 

![image](https://github.com/user-attachments/assets/f01ebb5f-2402-4e85-8100-0b679f254d86)

<b>*Note*: No additional information is required to be entered under the `Tag` tab</b>

- Navigate back to the Azure homepage or type in the search bar `Virtual Network` to create the network in which both Virtual Machines (Windows Server & Windows OS) will communicate. Select `Review + Create` until you get to the final tab.

![image](https://github.com/user-attachments/assets/8f91d0d7-d627-481f-85ca-20368f090887)

<b>*Note*: No additional information is required to be entered under the additional tabs</b>

- Create the Domain Controller (Windows Server 2022) VM by navgiating to the Azure homepage. <br>
      a) Select `Virtual Machine`

  ![image](https://github.com/user-attachments/assets/608e45a3-9426-4aa1-a20b-909759310cd5)
  ![image](https://github.com/user-attachments/assets/4978867e-f1f6-44da-b805-4e75fe5fa298)
  ![image](https://github.com/user-attachments/assets/8bbd2598-13c7-4d21-8502-361ff93b8674) <br>
      b) Navigate to the `Networking` tab to ensure that the virtual network created in the previous step is selected. Then select `Review + Create`.

  ![image](https://github.com/user-attachments/assets/9331e64b-1fba-4bb4-8b52-01becec0399d)

<b>2) Setup Client Computer</b>

- Navigate to the Azure homepage to create a `Virtual Machine`

![image](https://github.com/user-attachments/assets/5a319eb7-0c55-4197-8265-cff95e57a087)
![image](https://github.com/user-attachments/assets/f6653e04-a454-4f72-a67d-40311f56cc2e)
![image](https://github.com/user-attachments/assets/356fa969-c426-4ba1-9fc8-7709a1666151) <br>
    a) Navigate to the `Networking` tab to ensure that the virtual network created in the previous step is selected. Then select `Review + Create`.

![image](https://github.com/user-attachments/assets/b0a6e8ab-4864-410a-acf2-2889ddf6b1b2)

<b>3) Set Private IP Settings for Domain Controller to `Static`</b>

- Navigate to the Azure homepage and select the VM for the `Domain Controller`.
![image](https://github.com/user-attachments/assets/617fe3d9-c1df-4a25-8ccf-be6924bf452e)

- Select `Network Settings` > `Network Interface / IP Configuration` > `IP Configurations` > `ipconfig1` > Select `Static` under <b>Private IP address settings</b>

![image](https://github.com/user-attachments/assets/f7f073f3-9a4b-421d-ba6f-c4473c7887a0)

<b>4) Set Client Computer's DNS Settings to Domain Controller's Private IP Address</b>

- Navigate to the Azure homepage and select the VM for the `Client Computer`.

- Select `Network Settings` > `Network Interface / IP Configuration` > `DNS Servers` > `Custom` > Type in Domain Controller's Private IP Address under  <b>DNS server</b> > `Save`.

![image](https://github.com/user-attachments/assets/8352af8c-9529-43fb-9ade-7acda928333a)

![image](https://github.com/user-attachments/assets/9f7e62c4-68ca-4ff4-8125-4b58cff8ef81)

<b>5)Disable Domain Controller's Firewall Settings(for testing connectivity)</b>

- Open `Remote Desktop Connection` and enter Public IP Address for the Domain Controller Virtual Machine

![image](https://github.com/user-attachments/assets/c23e4c71-ea5f-4df8-90b2-5cc7b9ebc6e8)

<b>*Note*:</b> If not selected already, be sure to select the `Show Options` button to enter the VM account information (username & password) created earlier

- Open`Control Panel` from the start menu: <img src="https://github.com/user-attachments/assets/2cefedd8-2966-4817-ae8d-3ae3eee68ba5" height="3%" width="3%" alt="Windows Start Menu"/> and navigate to the following: `System & Security` > `Windows Defender Firewall` > Select `Turn Windows Defender On or Off`

![image](https://github.com/user-attachments/assets/eb443f86-c0f4-42bd-8633-1488599a1823)

- Under `Private & Public Network Settings` select `Turn off Windows Defender Firewall(not recommended)` > `OK`

![image](https://github.com/user-attachments/assets/f6486e69-6124-4721-8a27-74cde78a3046)

<b>6)Ping the Domain Controller from the Client Computer(for testing connectivity)</b>

- Restart the Client computer from the Azure Portal and login using `Remote Desktop Connection`
![image](https://github.com/user-attachments/assets/21398853-aa74-446d-a3ea-a2e2ac7ec6e6)
![image](https://github.com/user-attachments/assets/925309ff-bbae-4e8a-b1fb-8dfc6d232346)

- Open up Powershell from the start menu: <img src="https://github.com/user-attachments/assets/2cefedd8-2966-4817-ae8d-3ae3eee68ba5" height="3%" width="3%" alt="Windows Start Menu"/> and copy the Private IP for the Domain Controller into the ping command.
![image](https://github.com/user-attachments/assets/4144b78d-9cd3-4392-8b0d-46468c7cb8f4)

![image](https://github.com/user-attachments/assets/9d9c745a-ad49-4e48-b789-7fde8dc21c27)

<b>7)Verify the DNS Settings</b>

- Run `ipconfig/all` in Powershell, still logged into the Client computer to ensure the DNS points to the Domain Controller

![image](https://github.com/user-attachments/assets/5ca39a1a-1819-4fe2-a6aa-4a795eb22b5d)



<h3>Part 2: Installing and Deploying Active Directory</h3>

[![Video Title](https://img.youtube.com/vi/9SDWDI7HGvo/0.jpg)](https://youtu.be/9SDWDI7HGvo?si=KfihjFA-8LMuU134)

<b>1) Install Active Directory</b>

- Log into the `Domain Controller`

![image](https://github.com/user-attachments/assets/82ed4db5-330a-4897-b62c-02c8a977fb91)

- With the Server Manager Dashboard pulled up, select `Add Roles and Features`

![image](https://github.com/user-attachments/assets/12ecc9a6-73c7-4b97-a268-e06f71e7a95b)

- Continue to select `Next`until you reach the `Select server roles` tab > Select `Active Directory Domain Services`> `Add Features` > `Next`

![image](https://github.com/user-attachments/assets/6db9b187-2e5d-4878-8bf4-de6283b39520)

- Continue to select `Next` until you reach the `Confirm installation selections` page > Select `Restart the destination server automatically if required`> `Install`.

![image](https://github.com/user-attachments/assets/e8ce15b8-dcc7-4472-80d3-d43069ae30c2)

- Once installation is complete, navigate back to the `Server Manager` dashboard and select the flag to the left of the `Manage` tab on the top right-hand corner. Select `Promote this server to a domain controller`

![image](https://github.com/user-attachments/assets/d90ae286-ea9a-4928-aa89-a2f01dacd1f4)

- With the Active Directory Domain Services Configuration Wizard pulled up, select `Add a new forest` > Enter a`root domain name`> `Next`
![image](https://github.com/user-attachments/assets/ae5bdebc-b558-4821-a05a-80753027b7f4)

- Under the `Domain Controller Options` tab create and confirm a `Directory Services Restore Mode` password > `Next`
    <br>  <b>Note:</b> This will not be needed moving forward into the tutorial.

![image](https://github.com/user-attachments/assets/0ecefb89-f458-4956-9772-8c2ce563988a)

- Unselect the `Create DNS delegation` > `Next`

- Continue to select <b>Next</b> until you reach the `Installation` tab as the server gets ready to install AD > `Install`
    <br>  <b>Note</b>: The server will restart at this point, so go ahead and sign back into the Domain Controller

- Sign into the domain controller as `mydomain.com\yourusername` (ex.mydomain.com\cyberbaddy)

![image](https://github.com/user-attachments/assets/5a6881e6-3644-4aa3-9bab-c3b5183b24d3)

<b>2)Create a Domain Admin</b>

- Within the Domain Controller, select the `Start` menu: <img src="https://github.com/user-attachments/assets/2cefedd8-2966-4817-ae8d-3ae3eee68ba5" height="3%" width="3%" alt="Windows Start Menu"/> and search/open `Active Directory Users and Computers(ADUC)`

![image](https://github.com/user-attachments/assets/92de29e0-3144-4831-98cc-1585a8e15846)

- Create an Organizational Unit (OU) named `_EMPLOYEES` by right-clicking `mydomain.com` > New > `Organizational Unit`

![image](https://github.com/user-attachments/assets/a632140a-e205-4060-9db2-d9282a53c509)

- Repeat the step mentioned above to create another Organizational Unit named `_ADMINS`

- Create a new user within the `_ADMINS` folder and fill in the following fields: `Name`, `User Logon Name`, and `Password` > `Finish`

![image](https://github.com/user-attachments/assets/c6750787-d71d-4634-b2bc-5d1656c2368c)

- Add the newly created user: Domain Admin to the `Domain Admins` security group by navigating to the following:
       - `_ADMINS` > Right-click on `Username` > `Properties`> `MemberOf`

![image](https://github.com/user-attachments/assets/9eb96e76-fdf0-4984-8943-a554b741738f)

- With the `Select Groups` window pulled up, select `Add` and type `Domain Admins` under the field named `Enter the object names to select`> `Check Names`> OK > Apply > OK

![image](https://github.com/user-attachments/assets/6d7a579c-16ad-4ec6-a357-2da0c9278b1b)

<br> <b>Note:</b> Please make sure you select `Apply` before closing out the window

- Log out and log back in as `mydomain.com\`usernamehere`

![image](https://github.com/user-attachments/assets/9e1d342a-b0c8-440f-a216-137856d800c2)



<b>3)Join Client computer to Domain Controller</b>

- Login into the `Client` computer
![image](https://github.com/user-attachments/assets/a9408963-3505-4c82-8d08-0d9401260162)

- Navigate to the `Start` menu: <img src="https://github.com/user-attachments/assets/2cefedd8-2966-4817-ae8d-3ae3eee68ba5" height="3%" width="3%" alt="Windows Start Menu"/> and open `System` settings > `Rename this PC (advanced)`

![image](https://github.com/user-attachments/assets/10841f89-cd90-4120-9e49-735562c50f7a)

- Once the `System Properties` window is opened, select `Change` on the bottom right to open up the `Computer Name/Domain Changes` window
- Select `Domain` under the `Memberof` field > Type in root domain name> OK

![image](https://github.com/user-attachments/assets/910c9442-8470-460d-b627-7b44b81ce09c)


- Enter the username/password for the `Domain Admin` > OK

![image](https://github.com/user-attachments/assets/c263805b-13c1-40f2-9d2b-41c8d690e30f)

<br> <b>Note</b> To verify that the both the server and client computer are joined, log back into the `Domain Controller` and open up Active Directory. The Client computer name should appear under the organizational unit folder named `Computers`

![image](https://github.com/user-attachments/assets/00fb369a-5031-4be6-a915-ef74638f941a)

- Create a new `Organizational Unit` named `_Clients` by right-clicking on `mydomain.com`

![image](https://github.com/user-attachments/assets/33f781de-1c1e-4e71-8172-56345039aeb9)

- Move `Client-1` under the `Computers` folder to the newly created OU  `_CLIENTS`

![image](https://github.com/user-attachments/assets/15c5244b-6c9b-40b0-a02e-8c9f8fc6662d)



<h3>Part 3: Creating Non-Admin Users via Powershell</h3>

[![Video Title](https://img.youtube.com/vi/ozonZSqeWPM/0.jpg)](https://www.youtube.com/watch?v=ozonZSqeWPM)

<b>1)Setup Remote Desktop Session for Domain Users</b>

- Login into the `Client` computer as the `Domain Admin`

![image](https://github.com/user-attachments/assets/a3bf6cf2-86d2-46b3-8195-6c0d94031841)

- Open System Settings from the Windows start menu and select `Remote Desktop`> `Select sers that can remotely access this pc` > Add

![image](https://github.com/user-attachments/assets/3e07b07e-e0e8-44ee-8aa4-a30283df3984)

- With the `Select Users or Groups` window on display, <b>type</b> `Domain Users` under the field `Enter the object name to select`> `Check Names`> OK

![image](https://github.com/user-attachments/assets/6d9fa5c0-b8c7-4ea0-994f-fae9566daeea)


<b>2)Create Users with Powershell</b>










<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
