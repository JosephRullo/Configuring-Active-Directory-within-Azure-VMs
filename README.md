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

<h2>Step 4.</h2> Create the Client VM (Windows 10). Now it's time to set up the second virtual machine that will become the Client with which will connect to the Domain Controller via the network that was previously established. Go to the Azure home screen and select the Virtual Machine tab. Now select the same Resource Group that was created for the Domain Controller VM, (in this case ActiveDirectory). Leave the other settings the same as the DM setup except for the "Image", change that to Windows 10 operating system and for "Size" select 2 VCPUs and 16 GIB of memory. Assign a Username and Password and check the box at the very bottom under "Licensing" to confirm eligibility. Now you must go to the "Networking" tab (next to "Disks") and select the same "Virtual Network" that was created for the Domain Controller VM. You can view the Virtual Network on the VM Overview page we visited in the previous step. **Note sometimes the Virtual Network for the Domain Controller will not appear as an option to select for the Client, usually this is because the first VM that was created is still being built. This is ok, just wait a few minutes and refresh the browser and the next attempt at creating the Client VM should have the same Domain Controller's Virtual Network as the default already selected. Once this is done, you can validate and create the Client VM. Open both VM overview pages (Azure homepage -> Virtual Machine tab -> right click open each in a new browser) to ensure both Virtual Networks match and to make the next few steps a little easier to find the IP address' we will need to login.
<p>
<p>
<img src="https://imgur.com/boRURQ5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/jcsJAR0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/31GYCH7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 5.</h2> Ensure Connectivity between the client and Domain Controller. Go to the start menu and type "Remote Desktop" and the Remote Desktop Connection program. Log into the Client VM by copying the "Public IP Address" located on the VM overview screen -> enter it in the "Computer" field of Remote Desktop and click connect -> now enter the username and password that was assigned to it and click "Ok" to login -> click yes if a authentication warning appears. After logging in, go to the start menu withing the Client VM and type "CMD" to bring up "Command Prompt" select it to open. To test the connection with the Domain Controller VM that is on the same Virtual Network, we will send a ping via the command prompt. First take note of the "Private IP Address" for the Domain Controller on the VM overview page under the Networking section, this is the address we will ping (in this case 10.0.0.4). Type in the Command Prompt "ping -t 10.0.0.4" to send a perpetual ping (connection test) to our Domain Controller VM. Notice that the request is timed out. We will fix this in the next step.
<p>
<p> 
<img src="https://imgur.com/qNdcnwN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/MQXg0RI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 6.</h2> Ensure Connectivity between the client and Domain Controller (continued). Now we will login to our Domain Controller via Remote Desktop. Use the Public IP Address for the Domain Controller (found on VM overview screen) to enter in Remote Desktop and use the username and password for it (This may become a little confusing with two VM's open so it's best to have all the username's and password's written down and keep the window's seperated). Wait for Windows Server to finish loading -> go to the strart menu -> type in wf.msc to bring up the Windows Defender Firewall program and open it. In Windows Firewall -> select "Inbound Rules" -> locate the rule named "Virtual Machine Monitoring (Echo Request-ICMPv4-in) -> right click and "Enable Rule". Once this is complete, go back to the Client VM and notice now that you are receiving a reply from 10.0.0.4 (the Domain Controller). Great! Now we know there is a definite connection between the two computers.
<p>
<p> 
<img src="https://imgur.com/PjfKPz8.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/R2A27P9.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/x6h5Wp8.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 7.</h2> Install Active Directory. With connectivity established, can now begin to install Active Directory on the Domain Controller VM. On the Sever Manager Dasboard -> select 2. Add roles and features -> click "Next" in the wizard until you get to "Sever Roles" -> check the box next to "Active Directory Domain Services" -> click "Add Features" -> click "Next" until you get to confirmation -> click "Install" and wait. When the install is complete, you will see a yellow triangle appear in the top right corner next to a flag. Click this flag and a small window will appear under "Post-Deployment configuration" click on "Promote this server to a domain controller". A new window will open under Deployment Configuration -> select the deployment operation "Add a new forest" -> in the "Root domain name:" field assign it a name, in this example I chose "mydomain.com" (This will now be the Domain Controller's official name). Click "next" and enter in a password -> click "next" until you get to "Additional options" and wait for the domain name to load -> click next again until you come to the "Prerequisites check" section and click "Install". The Domain Controller VM will now restart. You will lose your connection to it in the process, but we will reconnect in the next step.
<p>
<p> 
<img src="https://imgur.com/Ee3ZbNU.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/bHrgjSx.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/WTWE1RG.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/mTH0zU6.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/WVaHLOb.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/SRO8aPJ.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/jIMTV0V.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/I2k5yty.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/syOapvK.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/LBvmgzt.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 8.</h2> Log back into the Domain Controller VM. To log back in we will now use the Domain Name we just created with the Active Directory install. Enter the domain's name you assigned followed by a backslash and then the Username, for this example it will be mydomain.com\labuser -> now enter the password and login. 
<p>
<p> 
<img src="https://imgur.com/5ZcDV4A.png" height="50%" width="50%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 9.</h2> Create an Admin and Normal User Account in Active Directory. Now that we are logged back into the Domain Controller VM under the new domain name we created, we will start to create groups that we can add users to. From the Sever Manager Dasboard in the upper right corner click on "Tools" -> select "Active Directory Users and Computers" (you can also go to the start menu and search for this as well). Once opened, Under the heading on the left of the window titled "Active Directory User and Computers" select the name of the domain that was created in the previous step(in this case mydomain.com). You will notice 6 folders that already exist here, we will add 2 more in this tutorial. Right click on the domain -> select New -> choose "Organizational Unit" -> name it "ADMINS" for this example. Repeat this and create another Organizational Unit called EMPLOYEES. 
<p>
<p> 
<img src="https://imgur.com/6JIbYfA.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/2AB3h38.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/fWsv9Z2.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 10.</h2> Create a new User and Administrator. Begin by opening the ADMINS organizational unit that was created in the previous step and right click -> select New -> select User. Fill in the First and Last name and assign a User Login Name -> click next -> assign a Password (uncheck "user must change password at next logon" for this example, usually this is left on) -> click Next -> click Finish (write down username and password in case you forget). Now we will make this User the Admin. To do this go to ADMINS -> right click on the User that was just created -> select Properties -> click the "Members of" tab -> click "Add" -> type "domain" in the "Enter the object names" field -> click "Check Names" -> select "Domain Admins" -> click Ok -> click Apply -> click Ok. Now we can logoff and sign back in as this new administator. Go to command prompt -> type logoff. Go back to remote desktop connection and login to the Domain Controller again this time using the domain name \  followed by the username and password we just assigned the admin, in this example it's mydomain.com\joe_admin. We will use this admin account for the Domain Controller moving forward.
<p>
<p> 
<img src="https://imgur.com/3967bDO.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/6Veye3B.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/CsKpwNk.png" height="70%" width="70%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/ZUD9JDh.png" height="50%" width="50%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 11.</h2> Join the Client to your Domain. In order for the Client VM to regonize the Domain we set up, we have to set the Domain Controller's DNS server as the Client's DNS server for it to work. Go to the Azure Portal -> go to the  Client VM's overview page -> click on Networking (on the right of the screen) -> click on the Network Interface (highlighted and bolded in blue) -> click "Choose DNS server" -> select "Custom" and type in the DNS server field the Private IP Address of the Domain Controller VM (can be found on the Domain Controller's VM overview page) -> click "Save". Once saving is complete, go back to the Azure Virtual Macnine page -> select the Client VM -> click Restart at the top of the screen -> click Yes. Wait a few minutes for the Client to restart, then log back into it (using the original username and password set for the Client VM) via Remote Desktop. Once logged back in to the Client -> right click on the Start menu -> select System -> click on "Rename this PC (advanced)" -> in the new window click "Change" next to change domain workgroup -> Click "Domain" under Member of -> enter the name of the Domain created in Active Directory in the field -> click Ok -> Enter in the Username and Password for assigned to the administrator of the Domain (ex mydomain\joe_admin) -> click Ok. The computer will prompt you to Restart (note prompt windows may be behind the window you are currently on). Click to restart. Now log on to the Client VM as the admin we created for the Domain Controller (we can do this now that we have joined the Client to the Domain). Lastly we will go back to the Domain Controller VM(Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain.

<p>
<p> 
<img src="https://imgur.com/5ZcDV4A.png" height="50%" width="50%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />
