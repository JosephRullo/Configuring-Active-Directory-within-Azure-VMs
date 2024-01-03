<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory Deployed in Microsoft's Cloud Platform (Azure)</h1>
This tutorial outlines the implementation of  Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join the Client to the Domain
- Setup Remote Desktop for non-administrative users on Client
- Create additional users and log into the Client with one of the users



<h2>Deployment and Configuration Steps</h2>
<p>
<p>
<h2>Step 1.</h2> **Azure VM** To begin sign in to Microsoft Azure by visiting (www.portal.azure.com) From here choose the "Virtual Machines" tab at the top. Now click "Create" -> select "Azure Virtual Machine".
<p>
<p>
<img src="https://imgur.com/BRZFE2w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 2.</h2> Create the Domain Controller Virtual Machine. The computer where Active Directory will be installed is known as the Domain Controller. From the Azure homescreen select the "Virtual Machines" tab at the top. Click on "Create" -> select "Azure Virtual Machine". From this screen you will start in the "Basics" tab. In the Resource Group field below select "Create New" and assign it a name of your choice (for example ActiveDirectory). This "Resource Group" is where the Virtual Machine will be stored. Next assign a name of your choice in the "Virtual Machine Name" field (ex. DomainController-VM), this will be the name of your Domain Controller moving forward. For this example I will leave the other settings default, except for the "Image" field. Set "Image" to "Windows Sever 2022 Datacenter: Azure Edition". Next scroll down and in the "Size" field select the option with 2vcpus and 16 Gib of memory. In the Administrator account section just below, assign a username and password (store in a secure place in case you forget it). Now we can click "Review+Create" at the very bottom. Once "Validation" has passed you can click "Create" once more at the bottom of the window. This may take a few minutes to complete.
<p>
<p>
<img src="https://imgur.com/ddZi8Wm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/CDAO9iB.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/oNnTmAZ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/09TbcTB.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 3.</h2> Set Domain Controller’s NIC Private IP address to be static.
<p>
<p>
<img src="https://imgur.com/QuZUmma.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
