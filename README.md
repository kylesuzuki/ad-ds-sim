<h1>Work in Progress (please check back later for updates).</h1>

<p align="center">
<img src="https://i.imgur.com/wHFQr4x.png" alt="Active Directory logo"/>
</p>

<h1>Active Directory Lab: User Authentication and Access Control Simulation in Azure</h1>
This beginner-friendly tutorial provides hands-on experience with setting up an on-premises Active Directory within Azure Virtual Machines. Learn how to install Active Directory on a Domain Controller VM and join a Client VM to the Domain. Explore essential tasks such as configuring Remote Desktop and managing user accounts. Practice proper resource management by deleting the resource group and verifying its successful removal.<br />

<h2>List Summary</h2>

- Setup Azure Resources and Establish Connectivity.
- Install and Configure Active Directory (AD).
- Create Admin and Normal User Accounts in AD.
- Join Client-1 to the Domain.
- Configure Remote Desktop for Non-Administrative Users.
- Create and Test Additional User Accounts.
- Delete the Resource Group to avoid incurring costs.
- Verify the successful deletion of the Resource Group.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computers)
- Remote Desktop
- Active Directory Domain Services
- Powershell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- Active <a href="https://azure.microsoft.com/en-us/free/">Azure account (Tenant) and Subscription</a>
- Completion of <a href="https://github.com/kylesuzuki/net-activities/tree/main">Performing Networking Activities within Azure VMs</a> (highly recommended)

<h2>Installation Steps</h2>

<h3>Setup Azure Resources and Establish Connectivity.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Azure Setup"/>
</p>
<p>
<ol>
  <li>In Azure, create a Domain Controller VM (Windows Server 2022) named "DC-1" and set its NIC Private IP address to Static.</li>
  <li>Create a Client VM (Windows 10) named "Client-1", ensuring that you use the same Resource group and Vnet as DC-1.</li>
  <li>Using Remote Desktop, log into Client-1 and initiate a perpetual ping to DC-1's private IP address.</li>
  <li>Open a second Remote Desktop window and log into DC-1 to enable ICMPv4.</li>
  <li>Monitor the ping from Client-1 to confirm its successful execution.</li>
</ol>
</p>

<h3>Install and Configure Active Directory (AD).</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Installation"/>
</p>
<p>
1. Install Active Directory Domain Services in DC-1.

2. Promote DC-1 to a Domain Controller & setup a new forest.
</p>

<h3>Create Admin and Normal User Accounts in AD.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Account Creation"/>
</p>
<p>
1. Open Active Directory Users and Computers (ADUC) and create two Organizational Units (OU) called "_EMPLOYEES" and "_ADMINS".

2. Create and add a new admin to the "Domain Admins" Security Group.
   
3. Log out of DC-1, then log back in as the new admin.
</p>

<h3>Join Client-1 to the Domain.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client to Domain"/>
</p>
<p>
1. In Azure, set Client-1's DNS settings to DC-1's NIC Private IP address and then restart Client-1.

2. Using Remote Desktop, log back into Client-1 as the original local admin and join Client-1 to the domain.
   
3. Back in DC-1, confirm that Client-1 is listed in ADUC.
</p>

<h3>Configure Remote Desktop for Non-Administrative Users.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="RD Configuration"/>
</p>
<p>
1. Using Remote Desktop, log back into Client-1 as the new admin and allow "domain users" access to Remote Desktop.
</p>

<h3>Create and Test Additional User Accounts.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Account Testing"/>
</p>
<p>
1. Using Remote Desktop, log back into DC-1 as the new admin and create additional users in Powershell. (code: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

2. Open a second Remote Desktop window and log into Client-1 with one of the generated user accounts.
   
3. Unlocking a user account with excessive incorrect password attempts.
   
4. Resetting a user account's password.
   
5. Disabling and Enabling a user account.
</p>

<h3>Delete the Resource Groups created earlier to avoid incurring costs.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Delete RG"/>
</p>
<p>
To delete the Resource Group, follow these steps: search "Resource Group", click on the resource group (e.g., **), click 'Delete resource group', then type or copy and paste the name of your resource group (e.g., RG-Lab-2) to confirm the deletion. Click 'Delete'.

Repeat these steps for the ** resource group that was automatically created earlier as well.
</p>

<h3>Verify the successful deletion of the Resource Groups.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Delete Verified"/>
</p>
<p>
To verify the successful deletion of the Resource Groups, search "Resource Group" and confirm that your specified resource groups (e.g., ** and **) are no longer listed.
</p>
