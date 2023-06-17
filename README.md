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
  <li>In Azure, create a Domain Controller VM (Windows Server 2022) named "DC-1" and set its NIC Private IP address to Static.
  <ul>
    <li>Start by searching for "Virtual Machine" and clicking on "Create." Choose your subscription and create a new resource group with a suitable name. Provide a name for the virtual machine, select the region, and set the availability options. Pick the desired Windows Server image and select an appropriate size for the virtual machine. Set a username and password, review the settings, and click "Create."</li>
    <li>To configure the networking for the virtual machine, search for "Virtual Machine" again and select the specific virtual machine. Access the "Networking" section, click on the network interface, and navigate to the IP configurations. Modify the assignment from "Dynamic" to "Static" to set a static IP address.</li>
  </ul></li>
  <li>Create a Client VM (Windows 10) named "Client-1", ensuring that you use the same Resource group and Vnet as DC-1.
      <ul>
    <li>Search for "Virtual Machine" and click on "Create." Choose your subscription, pick the resource group as "DC-1", and name the virtual machine. Select the region, set the availability options, and pick the desired Windows Server image. Select an appropriate size for the virtual machine, and set a username and password. Pick the same virtual network as "DC-1", review the settings, and click "Create."</li>
  </ul></li>
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
<ol>
  <li>Install Active Directory Domain Services in DC-1.</li>
  <li>Promote DC-1 to a Domain Controller and set up a new forest.</li>
</ol>
</p>

<h3>Create Admin and Normal User Accounts in AD.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Account Creation"/>
</p>
<p>
<ol>
  <li>Open Active Directory Users and Computers (ADUC) and create two Organizational Units (OU) called "_EMPLOYEES" and "_ADMINS".</li>
  <li>Create and add a new admin to the "Domain Admins" Security Group.</li>
  <li>Log out of DC-1, then log back in as the new admin.</li>
</ol>
</p>

<h3>Join Client-1 to the Domain.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client to Domain"/>
</p>
<p>
<ol>
  <li>In Azure, set Client-1's DNS settings to DC-1's NIC Private IP address and then restart Client-1.</li>
  <li>Using Remote Desktop, log back into Client-1 as the original local admin and join Client-1 to the domain.</li>
</ol>
</p>

<h3>Configure Remote Desktop for Non-Administrative Users.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="RD Configuration"/>
</p>
<p>
<ol>
  <li>Using Remote Desktop, log back into Client-1 as the new admin and allow "domain users" access to Remote Desktop.</li>
</ol>
</p>

<h3>Create and Test Additional User Accounts.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Account Testing"/>
</p>
<p>
<ol>
  <li>Using Remote Desktop, log back into DC-1 as the new admin and create additional users using this <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">code</a> in Powershell.</li>
  <li>Open a second Remote Desktop window and log into Client-1 with one of the generated user accounts.</li>
  <li>Unlock a user account with excessive incorrect password attempts.</li>
  <li>Reset a user account's password.</li>
  <li>Disable and enable a user account.</li>
</ol>
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
