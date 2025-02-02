<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Full Video Demonstrations</h2>

- ### [YouTube: How to Configure Active Directory Infrastructure within Azure](https://youtu.be/LLMdGsnudck) - Part 1

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com) - Part 2

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

[![Video Title](https://img.youtube.com/vi/LLMdGsnudck/0.jpg)](https://youtu.be/LLMdGsnudck?si=UQNqhanKu6ccev3g)






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
