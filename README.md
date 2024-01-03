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
<h2>Step 1.</h2> Azure VM. To begin sign in to Microsoft Azure by visiting (www.portal.azure.com) From here choose the "Virtual Machines" tab at the top. Now click "Create" -> select "Azure Virtual Machine".
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
<img src="https://imgur.com/ddZi8Wm.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/CDAO9iB.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/oNnTmAZ.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/09TbcTB.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 3.</h2> Set Domain Controller’s NIC Private IP address to be static. First go back to the homepage and select the "Virtual Machine" tab again and click on the domain controller VM that was just created. This will bring you to the VM Overview page where all of it's settings can be viewed and changed if neccessary. **Take note of the Resource Group and Virtual Network (Vnet) that were created for the VM (we will need this for the Client VM we will create next). On the left hand side under "Settings" click on "Networking". From the Networking page, next to the bold words "Network Interface:" you will see the virtual machine's network interface card highlighted and bolded in blue (in our example it is called "domaincontroller-vm204_z1"). Click on it and you will be brought to the "Network Interface Card" (NIC) settings page. Select "Configure your IP's"  button at the bottom of the screen. Now click on the name "ipconfig1", a new pop up will appear on the right of the screen. From there change the "Private IP Address Settings" from Dynamic -> to Static -> then click save. 
<p>
<p>
<img src="https://imgur.com/QuZUmma.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/s2u02zs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/BxU9mmr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/9cHazhQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/L5K3cwW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 4.</h2> Create the Client VM (Windows 10). Now it's time to set up the second virtual machine that will become the Client with which will connect to the Domain Controller via the network that was previously established. Go to the Azure home screen and select the Virtual Machine tab. Now select the same Resource Group that was created for the Domain Controller VM, (in this case ActiveDirectory). Leave the other settings the same as the DM setup except for the "Image", change that to Windows 10 operating system and for "Size" select 2 VCPUs and 16 GIB of memory. Now you must go to the "Networking" tab (next to "Disks") and select the same "Virtual Network" that was created for the Domain Controller VM. You can view the Virtual Network on the VM Overview page we visited in the previous step. **Note sometimes the Virtual Network for the Domain Controller will not appear as an option to select for the Client, usually this is because the first VM that was created is still being built. This is ok, just wait a few minutes and refresh the browser and the next attempt at creating the Client VM should have the same Domain Controller's Virtual Network as the default already selected. Once this is done, you can validate and create the Client VM. 
<p>
<img src="https://imgur.com/boRURQ5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/jcsJAR0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
