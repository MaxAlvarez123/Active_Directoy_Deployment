<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/56hP0ii.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/83vveQI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/zVxfv2M.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 1 : Setup Resources in Microsoft Azure

- In Microsoft Azure create two Virtual Machines.
    - One named "DC-1" and set its OS to Windows Server 2022.
    - The other one named "Client-1" and set its OS to Windows 10 and configure the network where Client-1 is using the same Vnet as DC-1.
    - Next you will change the IP Address of DC-1 from dynamic to static. DC-1 -> Networking -> Network Interface -> IP Configurations
</p>
<br />

<p>
<img src="https://i.imgur.com/r75R3fp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/hRyYU5m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2: Ensure connectivity between the client and Domain Controller

- Log into DC-1 using Remote Desktop and go into "Window Defender Firewall with Advance Security" -> Inbound Rules
- Once there enable the 3 ICMPv4 protocols highlighted in the picture above. 
- To make sure it has succesfully work log into Client-1 and ping the computer to the private IP Address.

</p>
<br />

<p>
<img src="https://i.imgur.com/RsNkruK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/D6dg9lq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/dIS411v.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/MeJRmNC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3: Install Active Directory

- In DC-1 go into server manager and click on "Add Roles and Features.
- Hit next until you get to the Server Roles section and add "Active Directory Domain Services".
- Continue clicking next until installation.
- After installation to continue click on the flag with the yellow exclamation point to complete the installation.
- Add a new forest and set the domain name to what you want. 
- Continue setting up untill you have intallation section and installed Active Directory.
- The VM will restart after installation
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/LYriSA8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/HO2nbjn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/f2t7Kew.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/0YlmQQt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: Create an Admin and Normal User Account in Active Directory 

- In Tools -> Active Directory Users and Computers create a Organizational Unit, for this tutorial we'll name it Employees. 
- Create another Orgnaizational Unit name Admins. 
- Create a new Admin named Hector Gonzalez and add Hector to the "Domain Admins" Security group. This gives complete control to the user over the domain server. 
- Next use the created account to login back into the DC-1 and use admin account from now on. 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/s2S2Wmp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/vthKOJR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/HPADzHm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 5: Join Client-1 to your domain

- From the Azure portal set Client-1's DNS setting to DC-1's Private IP address. Client-1 -> Networking -> Network Interface -> DNS Server.
- Set it to custom and paste the private IP address of DC-1. This will "point" to our DNS when joining it instead of trying to connect to the public internet's DNS.
- Restart Client-1 VM. 
- Go back into Client-1 with the credintials from Azure and go to settings -> About your PC -> Rename this PC(advnace) -> Change and type in your Domain. 
- After restarting the computer log back into Client-1 but this time with the admin account Hector. 

</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/rCXlFsu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6: Setup Remote Desktop for non-administrative users on Client-1

- Back to Client-1 go to setting -> remote destop setting -> select users that can remotely access this PC -> Add and type in Domain Users. This will make where everyone will be able to access this computer. 
- Your finally complete! 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/DrmnpHF.png" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 7 (optional): Create a bunch of additional users and attempt to log into client-1 with one of the users

- Go back to DC-1 and open powershell ice and as a administrator.
- Create and new file and paste the contents of this [script](https://docs.google.com/document/d/125TqjP753saKlv98uvSpBGr6cCFUAqJL7W8y7ymLYEs/edit) into in it.
- Run the script and log in as one of the users with the password being password1 
</p>
<br />
