<h1>Work in Progress (please check back later for updates).</h1>

<p align="center">
<img src="https://i.imgur.com/wHFQr4x.png" alt="Active Directory logo"/>
</p>

<h1>Active Directory Lab: User Authentication and Access Control Simulation in Azure</h1>
This comprehensive tutorial provides hands-on experience with setting up an on-premises Active Directory within Azure Virtual Machines. Learn how to install Active Directory on a Domain Controller VM and join a Client VM to the Domain. Explore essential tasks such as configuring Remote Desktop and managing user accounts. Practice proper resource management by deleting the resource group and verifying its successful removal.<br />

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
<h4>In Azure, create a Domain Controller VM (Windows Server 2022) named "DC-1" and configure its NIC Private IP address to Static.</h4>
<p>
<ol>
  <li>Search Virtual Machine</li>
    <ul>
      <li>A virtual machine is like a network switch that is in the cloud</li>
    </ul>
  <li>Click Create; Click Azure virtual machine</li>
  <li>Choose your Subscription (i.e. Azure subscription 1)</li>
  <li>For Resource group, click "Create new" and write a name (i.e. AD-Lab)</li>
  <li>Name your virtual machine (i.e. DC-1)
    <ul>
      <li>This stands for Domain Controller, which is just a server or a computer that has an active directory installed on it.</li>
    </ul>
  </li>
  <li>Pick a Region where the virtual machine is being created (i.e. (US) West US 3)</li>
  <li>Ensure Availability options is "No infrastructure redundancy required"</li>
  <li>Click Image and choose Windows Server 2022 Datacenter: Azure Edition - Gen2 (free services eligible)</li>
  <li>Click Size and choose Standard_E2s_v3 - 2 vcpus, 16 GiB memory ($159.14/month)</li>
  <li>Set Username (i.e. labuser)</li>
  <li>Set Password (i.e. Password1)</li>
  <li>Click Review + create
    <ul>
      <li>If you get a validation error for Networking, just click on the Networking tab and then go back to Review + create</li>
    </ul>
  </li>
  <li>Click Create
    <ul>
      <li>Take note of the Resource Group and Virtual Network (Vnet) that get created at this time</li>
    </ul>
  </li> 
  <li>Open DC-1</li>
  <li>Click Networking</li>
  <li>Click the Network Interface (aka NIC; should say something like dc-1755)</li>
  <li>Click IP configurations</li>
  <li>Click ipconfig1</li>
  <li>Change Assignment from Dynamic to Static
    <ul>
      <li>Static means the IP address is always going to be this and it is not going to change (i.e. regardless of if we turn the computer off and leave it off for like a year and turn it back on).</li>
    </ul>
  </li>
  <li>Click Save</li>
</ol>
</p>
<h4>Create a Client VM (Windows 10) named "Client-1", ensuring that you use the same Resource group and Vnet as DC-1.</h4>
<p>
<ol>
  <li>Search Virtual Machine</li>
  <li>Click Create; Click Azure virtual machine</li>
  <li>Choose your Subscription (i.e. Azure subscription 1)</li>
  <li>Choose your resource group (i.e. AD-Lab)</li>
  <li>Name your virtual machine (i.e. Client-1)</li>
  <li>Pick a Region where the virtual machine is being created (i.e. (US) West US 3)</li>
  <li>Ensure Availability options is “No infrastructure redundancy required”</li>
  <li>Click Image and choose Windows 10 Pro, version 21H2 - Gen2 (free services eligible)</li>
  <li>Click Size and choose Standard_E2s_v3 - 2 vcpus, 16 GiB memory ($91.98/month)</li>
  <li>Set Username (i.e. labuser)</li>
  <li>Set Password (i.e. Password1)</li>
  <li>Click Next; Leave all the Disk options as it is; Click Next</li>
  <li>Ensure Virtual network is set to DC-1-vnet</li>
  <li>Ensure Subnet is set to default (10.0.0.0/24)</li>
  <li>Wait for validation on Review + create then click create</li>
  <li>Click Review + create</li>
  <li>Click Create</li>
</ol>
</p>
<h4>Using Remote Desktop, log into Client-1 and initiate a perpetual ping to DC-1's private IP address.</h4>
<p>
<ol>
  <li>Open Client-1</li>
  <li>Copy the Public IP address</li>
  <li>Open the Remote Desktop Connection application on your computer</li>
  <li>Paste the Public IP address</li>
  <li>Click Connect</li>
  <li>Enter credentials (i.e. Username: labuser and Password: Password1)
    <ul>
      <li>If you do not see an option to enter credentials, click More choices, then click Use a different account.</li>
    </ul>
  </li>
  <li>A warning message may pop up saying that it’s not trustworthy, just click Yes</li>
  <li>A "Choose privacy settings for your device" message may pop up, just set all to No and click accept</li>
  <li>Back on your actual computer, Search Virtual Machine</li>
  <li>Click DC-1</li>
  <li>Copy the Private IP address</li>
  <li>Go back into your Client-1 Remote Desktop Connection</li>
  <li>Open Command Prompt</li>
  <li>Type "ping -t (and paste the private IP address)" then press enter
    <ul>
      <li>ping -t is a perpetual ping that will keep going forever until you stop it</li>
      <li>Notice the requests time out because DC-1’s Windows Firewall is blocking ICMP traffic.</li>
    </ul>
  </li>
  <li>Press Ctrl + C to stop the perpetual ping</li>
</ol>
</p>
<h4>Open a second Remote Desktop window and log into DC-1 to enable ICMPv4.</h4>
<p>
<ol>
  <li>Back in Azure, open DC-1</li>
  <li>Copy the Public IP address</li>
  <li>Open another window of the Remote Desktop Connection application on your computer</li>
  <li>Paste the Public IP address</li>
  <li>Click Connect
    <ul>
      <li>Note: If you ever get confused which remote desktop connection window is DC-1 or Client-1, you can open the Command Prompt, type "hostname", and click enter to see which one you are in.</li>
    </ul>
  </li>
  <li>Enter credentials (i.e. Username: labuser and Password: Password1)</li>
  <li>A warning message may pop up saying that it’s not trustworthy, just click Yes</li>
  <li>Once logged in, in the "Type here to search" section (bottom left corner of the screen), search wf.msc and open the search result
    <ul>
      <li>wf stands for Windows Firewall; msc stands for Microsoft Management Console</li>
      <li>Alternatively, search Windows Defender Firewall with Advanced Security and open search result</li>
    </ul>
  </li>
  <li>The Server Manager application should automatically pop up. If it doesn’t, you can search Server Manager in the "Type here to search" section (bottom left corner of the screen) and open the search result</li>
  <li>Click Inbound Rules</li>
  <li>Click Protocol (to sort the inbound rules by protocol type)</li>
  <li>Look for the following two inbound rules named "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" using ICMPv4 protocol and right click them, then click Enable rule</li>
    <ul>
      <li>ICMP is the protocol that ping uses, so we are allowing Client-1 to be able to ping DC-1</li>
    </ul>
</ol>
</p>
<h4>Monitor the ping from Client-1 to confirm its successful execution.</h4>
<p>
<ol>
  <li>Go back into your Client-1 Remote Desktop Connection</li>
  <li>Open Command Prompt</li>
  <li>Type "ping -t (and paste the private IP address)" then press enter</li>
    <ul>
      <li>Notice the requests are no longer timed out because DC-1’s Windows Firewall is no longer blocking ICMP traffic.</li>
    </ul>
  <li>Press Ctrl + C to stop the perpetual ping.</li>
</ol>
</p>

<h3>Install and Configure Active Directory (AD).</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Installation"/>
</p>
<h4>Install Active Directory Domain Services in DC-1.</h4>
<p>
<ol>
  <li>Go back into your DC-1 Remote Desktop Connection</li>
  <li>In the Server Manager application, click Add roles and features</li>
    <ul>
      <li>This is how we install Active Directory</li>
    </ul>
  <li>Click Next (x3 times)</li>
  <li>Click the box next to Active Directory Domain Services to check it</li>
    <ul>
      <li>A pop-up screen may appear, just click Add Features</li>
    </ul>
  <li>Click Next (x3 times)</li>
  <li>Click Install</li>
  <li>Once installed, click Close</li>
</ol>
</p>
<h4>Promote DC-1 to a Domain Controller and set up a new forest.</h4>
<p>
<ol>
  <li>In the Service Manager application, click on the flag with an exclamation symbol on the top right corner of the screen.</li>
  <li>Click Promote this server to a domain controller
    <ul>
      <li>This is how we actually finish installing Active Directory and turn the server into a domain controller.</li>
    </ul>
  </li>
  <li>Click Add a new forest
    <ul>
      <li>This is how we name the domain.</li>
    </ul>
  </li>
  <li>Enter a Root domain name (i.e. mydomain.com)</li>
  <li>Click Next</li>
  <li>Enter a Password (i.e. Password1)</li>
  <li>Click Next (x5 times)</li>
  <li>Click Install</li>
  <li>Allow the remote desktop connection to automatically restart (you will need to reconnect)</li>
  <li>Open another window of the Remote Desktop Connection application on your computer and log back into DC-1
    <ul>
      <li>Notice that your credentials (i.e. Username: labuser and Password: Password1) no longer work because DC-1 is a domain controller now. So...</li>
        <ul>
          <li>Click "Use a different account"</li>
          <li>For "Email address," enter the FQDN (aka Fully Qualified Domain Name; i.e. mydomain.com\labuser)</li>
          <li>For Password, enter your password (i.e. Password1)</li>
        </ul>
    </ul>
  </li>
  <li>A warning message may pop up saying that it’s not trustworthy, just click Yes</li>
</ol>
</p>

<h3>Create Admin and Normal User Accounts in AD.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="AD Account Creation"/>
</p>
<h4>Open Active Directory Users and Computers (ADUC) and create two Organizational Units (OU) called "_EMPLOYEES" and "_ADMINS".</h4>
<p>
<ol>
  <li>In the Service Manager application, click on Tools, then click Active Directory Users and Computers
    <ul>
      <li>Alternatively, in the "Type here to search" section (bottom left corner of the screen), search Active Directory Users and Computers and open search result</li>
      <li>Alternatively, in the "Type here to search" section (bottom left corner of the screen), open the "Windows Administrative Tools" folder then click Active Directory Users and Computers</li>
    </ul>
  </li>
  <li>Right click mydomain.com, go to New, go to Organizational Unit
    <ul>
      <li>For now, you can think of Organizational Units like folders</li>
    </ul>
  <li>Enter a name (i.e. _EMPLOYEES)</li>
  <li>Click OK</li>
  <li>Right click mydomain.com again, go to New, go to Organizational Unit
  <li>Enter a name (i.e. _ADMINS)</li>
  <li>Click OK</li>
</ol>
</p>
<h4>Create and add a new admin to the "Domain Admins" Security Group.</h4>
<p>
<ol>

</ol>
</p>
<h4>Log out of DC-1, then log back in as the new admin.</h4>
<p>
<ol>

</ol>
</p>

<h3>Join Client-1 to the Domain.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Client to Domain"/>
</p>
<h4>In Azure, set Client-1's DNS settings to DC-1's NIC Private IP address and then restart Client-1.</h4>
<p>
<ol>

</ol>
</p>
<h4>Using Remote Desktop, log back into Client-1 as the original local admin and join Client-1 to the domain.</h4>
<p>
<ol>

</ol>
</p>

<h3>Configure Remote Desktop for Non-Administrative Users.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="RD Configuration"/>
</p>
<h4>Using Remote Desktop, log back into Client-1 as the new admin and allow "domain users" access to Remote Desktop.</h4>
<p>
<ol>

</ol>
</p>

<h3>Create and Test Additional User Accounts.</h3>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Account Testing"/>
</p>
<h4>Using Remote Desktop, log back into DC-1 as the new admin and create additional users using this <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">code</a> in Powershell.</h4>
<p>
<ol>

</ol>
</p>
<h4>Open a second Remote Desktop window and log into Client-1 with one of the generated user accounts.</h4>
<p>
<ol>

</ol>
</p>
<h4>Unlock a user account with excessive incorrect password attempts.</h4>
<p>
<ol>

</ol>
</p>
<h4>Reset a user account's password.</h4>
<p>
<ol>

</ol>
</p>
<h4>Disable and enable a user account.</h4>
<p>
<ol>

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
