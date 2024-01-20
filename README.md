<p align="center">
<img src="https://i.imgur.com/QpoJl8Y.png" height=120% width=120% alt="Microsoft Active Directory Logo"/>
</p>

<h1> <p align="center"> Active Directory Deployed in Microsoft's Cloud Platform (Azure)</h1>
<p align="center"> This tutorial outlines the implementation of  Active Directory within Azure Virtual Machines.<br />
<br />
<br />
<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure
- Virtual Machines
- Remote Desktop
- Windows Defender Firewall
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
- Setup Remote Desktop for Non-Administrative Users on Client
- Create Additional Users and Log into the Client with one of the Users



<h2>Deployment and Configuration Steps</h2>
<p>
<p>
<h2>Step 1.</h2> 
  
**Microsoft Azure.** 
<p>
To begin visit (www.portal.azure.com) -> create an account -> sign in.
<p>
<p> 
<img src="https://i.imgur.com/vhHZHOS.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 2.</h2> 

**Create the Domain Controller Virtual Machine.** 
<p>
The computer where Active Directory will be installed is known as the Domain Controller. From the Azure homescreen select the "Virtual Machines" tab at the top. Click on "Create" -> select "Azure Virtual Machine". From this screen you will start in the "Basics" tab. In the Resource Group field below select "Create New" and assign it a name of your choice (for example ActiveDirectory). This "Resource Group" is where the Virtual Machine and it's resources will be stored. Next assign a name of your choice in the "Virtual Machine Name" field (ex. DomainController-VM), this will be the name of your Domain Controller moving forward. For this example I will leave the other settings default, except for the "Image" field. Set "Image" to "Windows Sever 2022 Datacenter: Azure Edition". Next scroll down and in the "Size" field select the option with 2vcpus and 16 Gib of memory. In the Administrator account section just below, assign a username and password (store in a secure place in case you forget it). Now we can click "Review+Create" at the very bottom. Once "Validation" has passed you can click "Create" once more at the bottom of the window. This may take a few minutes to complete.
<p>
<p>
<img src="https://i.imgur.com/P5NesqQ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/XisNYIb.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/5Tr1OM0.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/9kl2fML.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/T5b2S0X.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 3.</h2> 

**Set Domain Controller’s NIC Private IP Address to Static.**
<p>
Go back to the Azure homepage and select the "Virtual Machine" tab again and click on the domain controller VM that was just created. This will bring you to the VM Overview page where all of it's settings can be viewed and configured as needed. Take note of the "Resource Group" and "Virtual Network (Vnet)" that were created for the VM (we will need this for the Client VM that will be created next). On the left hand side under "Settings" click on "Networking". From the Networking page, next to the bold words "Network Interface:" you will see the virtual machine's "Network Interface Card" highlighted and bolded in blue (in our example it is called "domaincontroller-vm204_z1"). Click on it and you will be brought to the "Network Interface Card" (NIC) settings page. Select "Configure your IP's"  button at the bottom of the screen. Now click on the name "ipconfig1", a new pop up will appear on the right of the screen. From there change the "Private IP Address Settings" from Dynamic -> to Static -> then click save. 
<p>
<p>
<img src="https://i.imgur.com/XmflLY2.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/xzBBqR4.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/bI6Kea5.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/oM0Yx86.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/7FiBZZO.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Tq52H7p.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 4.</h2>

**Create the Client VM (Windows 10).** 
<p>
Now it's time to set up the second virtual machine that will become the Client. This VM will connect to the Domain Controller with the Virtual Network that was previously established. Go to the Azure home screen -> select the Virtual Machine tab -> select the same "Resource Group" that was created for the Domain Controller VM (in this case ActiveDirectory). Leave the other settings the same as the Domain setup except for the "Image" -> change that to Windows 10 operating system -> go to "Size" -> select 2 VCPUs and 16 GIB of memory -> Assign a Username and Password -> check the box at the very bottom under "Licensing" to confirm eligibility. Now you must go to the "Networking" tab (next to "Disks") and select the same "Virtual Network" that was created for the Domain Controller VM. You can view the Virtual Network on the VM Overview page we visited in the previous step. (**Note sometimes the Virtual Network for the Domain Controller will not appear as an option to select for the Client, usually this is because the first VM is still in the process of being created in the cloud. This is ok, just wait a few minutes and refresh the browser and the next attempt at creating the Client VM should have the same Domain Controller's Virtual Network as the default already selected.) Once this is done, you can validate and create the Client VM.
<p>
<p>
<img src="https://i.imgur.com/4aQdMdw.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/1ghWmpy.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/bacuJNE.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 5.</h2>

**VM Overview** 
<p>
To make it easy to find the IP Addresses we will need to copy in the coming steps, let's open the Overview pages for the two VM's we just created. Go to the Azure homepage -> click the Virtual Machine tab -> hold down the ctrl button on the keyboard and left click each Virtual Machine highlighted in blue. Two new browser windows will open with each VM's Overview of all their configuration settings. At this time ensure both "Virtual Networks" match (located in the upper right corner of the overview page), in this case DomainController-VM-vnet/default. Also take note of the Public and Private IP Addresses for both VM's.
<p>
<p>
<img src="https://i.imgur.com/S5bQeLb.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/ELJS48D.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 6.</h2>

**Notepad (Optional)** 
<p>
This step is optional, but will be very helpful for keeping track of all the Usernames and Passwords that will be created throughout this tutorial. Go to the start menu and type "Notepad" and open the app. Write down the usernames and passwords for the Domain and Client VM's that were created. You can add to this list as you create more later on. *Note for keeping track of real world passwords and sensitive login info, use a password manager client or another secure method.  
<p>
<p>
<img src="https://i.imgur.com/DneaCuI.png" height="3%" width="30%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 7.</h2> 

**Login to the Client Virtual Machine with Remote Desktop.** 
<p>
Go to the start menu and type "Remote Desktop" and open the Remote Desktop Connection program. Log into the Client VM by copying the "Public IP Address" located on the Client VM overview page (upper right corner) -> enter it in the "Computer" field of Remote Desktop and click connect -> now enter the username and password that was assigned to it and click "Ok" to login -> click yes if an authentication warning appears.
<p>
<p> 
<img src="https://i.imgur.com/IeWeNBe.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 8.</h2>

**Ensure Connectivity between the Client and Domain Controller.** 
<p>
After logging in, a new window will open. This new window is your Client Virtual Machine. You can minimize this window anytime to return to the "Host" computer. From within that Client VM window go to the start menu and type "CMD" to bring up "Command Prompt" -> click it to open. To test the connection with the Domain Controller VM (that is on the same Virtual Network), we will send a "Ping" (connection test) via the command prompt. First take note of the "Private IP Address" for the Domain Controller on it's VM overview page (under the Networking section on the right). This is the address we will "Ping" (in this case it's 10.0.0.4). Type in the Command Prompt "ping -t 10.0.0.4" to send a perpetual "ping" to our Domain Controller VM. Notice that the request is timed out. We will fix this in the next step.
<p>
<p> 
<img src="https://imgur.com/MQXg0RI.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 9.</h2> 

**Ensure Connectivity between the client and Domain Controller (continued).** 
<p>
Now we will login to our Domain Controller via Remote Desktop. Use the Public IP Address for the Domain Controller and log in Remote Desktop using the username and password assigned to it. Wait for the Windows Server to finish loading -> go to the strart menu -> type in wf.msc to bring up the Windows Defender Firewall program and open it. In Windows Firewall -> select "Inbound Rules" -> locate the rule named "Virtual Machine Monitoring (Echo Request-ICMPv4-in) -> right click and "Enable Rule". Once this is complete, go back to the Client VM window and notice now that you are receiving a reply from 10.0.0.4 (the Domain Controller). You can stop the ping by hitting ctrl + c on the keyboard. Great! Now we know there is a definite connection between the two computers.
<p>
<p> 
<img src="https://i.imgur.com/HQNvhES.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/za38HqA.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/x6h5Wp8.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 10.</h2>

**Install Active Directory.** 
<p>
With connectivity established, we can now begin to install Active Directory on the Domain Controller VM. On the Sever Manager Dasboard -> select 2. Add roles and features -> click "Next" in the wizard until you get to "Sever Roles" -> check the box next to "Active Directory Domain Services" -> click next-> click "Add Features" -> click "Next" until you get to confirmation -> click "Install" and wait.
<p>
<p> 
<img src="https://i.imgur.com/ylaPPZT.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/9OpDlke.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/HVg7Y3c.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/Hd23LXj.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 11.</h2>

**Install Active Directory (continued).** 
<p>
When the install is complete, you will see a yellow triangle appear in the top right corner next to a flag. Click this flag and a small window will appear. Under "Post-Deployment configuration" click on "Promote this server to a domain controller" -> a new window will open. Under Deployment Configuration -> select the deployment operation "Add a new forest" -> in the "Root domain name:" field assign it a name, in this example I chose "mydomain.com" (This will now be the Domain Controller's official name). Click "next" and enter in a password (Note this is a "Restore Mode" password, we will not be using this for logging in. It's neccessary to enter one to complete the install.)  -> click "next" until you get to "Additional options" and wait for the domain name to load -> click next again until you come to the "Prerequisites check" section and click "Install". The Domain Controller VM will now restart. You will lose your connection to it in the process, that's ok, we will reconnect in the next step.
<p>
<p> 
<img src="https://i.imgur.com/mSynPbJ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/ZPAZ7nc.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/lTKPk1m.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/dwAS3MJ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/hLebJiZ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/LBvmgzt.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 12.</h2> 

**Log back into the Domain Controller VM.** 
<p>
To log back in we will now use the Domain Name we just created with the Active Directory install. Enter the domain's name you assigned followed by a backslash and then the Username, for this example it will be mydomain.com\labuser -> now enter the Domain VM's password and login. 
<p>
<p> 
<img src="https://i.imgur.com/TDfEIQg.png" height="40%" width="40%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 13.</h2> 

**Create an Admin and Normal User Account in Active Directory.**
<p>
Now that we are logged back into the Domain Controller VM under the new domain name we created, we will start to create groups that we can add users to. From the Sever Manager Dasboard in the upper right corner click on "Tools" -> select "Active Directory Users and Computers" (you can also go to the start menu and search for this as well). Once opened, Under the heading on the left of the window titled "Active Directory User and Computers" select the name of the domain that was created in the previous step (in this case mydomain.com). You will notice 6 folders that already exist here, we will add 2 more in this tutorial. Right click on the domain -> select New -> choose "Organizational Unit" -> name it "ADMINS" for this example. Repeat this and create another Organizational Unit called EMPLOYEES. 
<p>
<p> 
<img src="https://i.imgur.com/uDhbXrj.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Y1ul5nM.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/2AB3h38.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/LEuM3UX.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 14.</h2>

**Create a new User/Administrator.**
<p>
Begin by opening the ADMINS organizational unit that was created in the previous step and right click -> select New -> select User. Fill in the First and Last name and assign a User Login Name -> click next -> assign a Password (uncheck "user must change password at next logon" for this example, usually this is left on) -> click Next -> click Finish (write down username and password in case you forget).
<p>
<p> 
<img src="https://i.imgur.com/yLhBpqI.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/6Veye3B.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 15.</h2>

**Create a new User/Administrator (continued).**
<p>
Now we will make this User the Admin. To do this go to ADMINS -> right click on the User that was just created -> select Properties -> click the "Members of" tab -> click "Add" -> type "domain" in the "Enter the object names" field -> click "Check Names" -> select "Domain Admins" -> click Ok -> click Apply -> click Ok. Now we can logoff and sign back in as this new administator. Go to command prompt -> type logoff. Go back to Remote Desktop Connection and login to the Domain Controller again this time using the domain name \  followed by the username and password we just assigned the Admin, in this example it's mydomain.com\joe_admin. We will use this Admin account for the Domain Controller moving forward.
<p>
<p> 
<img src="https://i.imgur.com/uNLgrhK.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Au9BCme.png" height="40%" width="40%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 16.</h2> 

**Join the Client to your Domain.** 
<p>
In order for the Client VM to regonize the Domain we set up, we have to set the Domain Controller's DNS server as the Client's DNS server for it to work. Go to the Azure Portal -> go to the  Client VM's overview page -> click on Networking (on the left of the screen) -> click on the Network Interface: (highlighted and bolded in blue) -> click "Choose DNS server" -> select "Custom" and type in the DNS server field the "Private IP Address" of the Domain Controller VM (found on the Domain Controller's VM overview page) -> click "Save". Once saving is complete, go back to the Azure Virtual Machine page -> select the Client VM -> click Restart at the top of the screen -> click Yes. Wait a few minutes for the Client to restart.
<p>
<p> 
<img src="https://i.imgur.com/zeJy8Gt.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/WpOJkqs.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/wxO3Ur2.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/S5Olu1s.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<p>
</p>
<br />

<h2>Step 17.</h2> 

**Join the Client to your Domain(continued).** 
<p>
Log back into the Client VM (using the original username and password set for it) via Remote Desktop. Once logged back in -> right click on the Start menu -> select System -> click on "Rename this PC (advanced)" -> in the new window click "Change" next to change domain workgroup -> Click "Domain" under Member of -> enter the name of the Domain "created in Active Directory" in the field -> click Ok -> Enter in the Username and Password assigned to the Administrator of the Domain -> click Ok. The computer will prompt you to Restart (Prompt windows may be behind the window you are currently on, click and drag them aside if needed). Click to restart. Now log on to the Client VM as the Admin we created for the Domain Controller. We can do this now that we have joined the Client to the Domain.
<p>
<p>  
<img src="https://i.imgur.com/O2io8A0.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/jZ18EuW.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/DYY0d3d.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/3x10UXA.png" height="40%" width="40%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 18.</h2>

**Verify Client is Present in Active Directory.** 
<p> Switch back to the Domain Controller VM and verify Client shows up in Active Directory. Go to Active Directory Users and Computers (ADUC) -> select the Domain -> select “Computers”. You should see the Client VM present in this group.
<p>
<p>  
<img src="https://i.imgur.com/fQB451I.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
</p>
<br />

<h2>Step 19.</h2> 

**Setup Remote Desktop for Non-Administrative Users on Client VM.** 
<p>
Logon to the Client VM as the Domain Admin. From the home screen, right click on the start menu -> select system -> select Remote Desktop (over to the right) -> under User Accounts click "Select users that can remotely access this PC" -> Click Add -> type "domain users" in the "Enter the object names" field -> click Check Names -> select Domain Users -> click Ok -> click Ok again.
<p>
<p> 
<img src="https://i.imgur.com/QErYrob.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/tvAF0F6.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

<h2>Step 20.</h2>

**Create Users to Test Functionality.** 
<p>
Finally let's create some new users in Active Directory. We will test logging on the Client with them, ensuring everything is working properly. Log into the Domain Controller VM as the Admin -> open Active Directory Users and Computers -> select EMPLOYEES -> right click select New -> select User. You can create any number of users you like and assign any name/user logon/password to them, as this is only a test. Once this is complete, attempt to log into the Client VM using the Username and Password from one of the users you just created (in this example mydomain.com\alice.a). 
<p>
<p> 
<img src="https://i.imgur.com/EfKgP90.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/dBo5aDL.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/kPAxDDM.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/8epTMKs.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/T2veW0B.png" height="40%" width="40%" alt="Disk Sanitization Steps"/> 
<img src="https://imgur.com/rNdiOTy.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
<h2>Step 21.</h2>

**Create Users with PowerShell (Alternative Method)(Optional)** 
<p>
For a more robust test with User logins, password changes and network sharing, let's run a script utilizing PowerShell. PowerShell is made up of a command-line shell, a scripting language, and a configuration management framework that runs on Windows, Linux, and macOS. The prewritten script we will borrow will create thousands of random Users that will be able to login to the client all with the same password. Go to the start menu and type PowerShell -> right click and run as Administrator. Once open click on "New Script" at the top left -> copy and paste this script located here https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 -> Modify the Script (optional, in this case I modified it to create 5000 Users instead of 10000 and changed the Password) -> click "Run Script". This will take a few minutes to complete. Once finished, you can go back to Active Directory Users and Computers -> right click on _EMPLOYESS and select "Refresh". Now you should see all the new random users that the script created for us. You can select any one of them and follow step 20 to log in with them as a regular User.
<p>
<p> 
<img src="https://i.imgur.com/MEN9WoP.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/Hh1lrDb.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/KlH2B07.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/YnDmD1p.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/QzlxnZa.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
<img src="https://i.imgur.com/fF0MIga.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
<h2> <p align="center">  To learn how to share access to folders and set various permissions in Active Directory please view this tutorial: 
<p> <p align="center"> https://github.com/JosephRullo/Active-Directory-Security-Groups-and-Sharing-Access-Permissions/blob/main/README.md. </h2>
<h2> <p align="center"> Active Directory Setup is now Complete!!! </h2>
