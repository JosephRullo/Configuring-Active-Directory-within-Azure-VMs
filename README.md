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
<h2>Step 1.</h2> To begin sign in to Microsoft Azure by visiting (www.portal.azure.com) From here choose the "Virtual Machines" tab at the top. Now click "Create" -> select "Azure Virtual Machine".
<p>
<p>
<img src="https://imgur.com/BRZFE2w.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 2.</h2> Create the Domain Controller Virtual Machine. The computer where Active Directory will be installed is known as the Domain Controller. From the Azure homescreen select the "Virtual Machines" tab at the top. Click on "Create" -> select "Azure Virtual Machine". From this screen you will start in the "Basics" tab. Select "Create new" in the Resource Group field (This is where the Virtual Machine will be stored) and assign it a name of your choice (ie ActiveDirectory). Next assign a name of your choice in the "Virtual Machine Name" field (ie DomainController-VM), this will be the name of your Domain Controller moving forward.
<p>
<p>
<img src="https://imgur.com/ddZi8Wm.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/CDAO9iB.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/oNnTmAZ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />
