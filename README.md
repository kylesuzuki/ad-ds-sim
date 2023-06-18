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
- Delete the Resource Groups created in this lab to avoid incurring costs and verify its deletion.

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
<img src="https://i.imgur.com/T5POc14.png" height="80%" width="80%" alt="Azure and Connectivity"/>
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
      <li>If you ever get confused which remote desktop connection window is DC-1 or Client-1, you can open the Command Prompt, type "hostname", and click enter to see which one you are in.</li>
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
<img src="https://i.imgur.com/oZUV1jl.png" height="80%" width="80%" alt="AD Installation"/>
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
<img src="https://i.imgur.com/9kISnyR.png" height="80%" width="80%" alt="AD Account Creation"/>
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
<ul>
  <li>Normally when you install Active Directory, it’s usually done with some kind of generic (administrator) account (i.e. "labuser"). However, you want to get out of the habit of using these generic accounts to do administrative tasks.
    <ul>
      <li>You will notice when you start working somewhere, you will usually have more than one account and then everyone will have their own accounts too. It is really bad practice to have an account called "user" or "admin" because account names should always be tied to a human’s identity.</li>
      <li>So since we have a kind of ambiguous and not really realistic account called "labuser" in the Users Organizational Unit, we’re going to create another administrator account that’s tied to us as an individual (i.e. Jane Doe) and then we’ll log out of "labuser" and log back in with that new administrator account.
        <ul>
          <li>We could keep using "labuser" but again, creating our own admin account will give us some kind of idea of actual real-world practice.</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
</p>
<p>
<ol>
  <li>Right click _ADMINS, go to New, go to Organizational Unit
  <li>Enter First name (i.e. jane)</li>
  <li>Enter Last name (i.e. doe)</li>
  <li>Enter User logon name (i.e. jane_admin)
    <ul>
      <li>A lot of times in organizations, you’ll see something like "a-jane", which means "admin-jane"</li>
    </ul>
  </li>
  <li>Click Next</li>
  <li>Enter Password (i.e. Password1)</li>
  <li>Uncheck "User must change password at next logon"
    <ul>
      <li>Normally, we’d want to leave this checked and force the user to change it, but we’re just creating it for ourselves.</li>
    </ul>
  </li>
  <li>Check "Password never expires"</li>
  <li>Click Next</li>
  <li>Click Finish
    <ul>
      <li>Now a user account named jane_admin has been created, but it’s not actually an admin yet.</li>
    </ul>
  </li>
  <li>In the _ADMINS organizational unit, right click jane_admin, click on Properties.</li>
  <li>Click Member Of
    <ul>
      <li>Notice this user (jane_admin) is a member of "Domain Users" group, which is a default group that all domain accounts are a member of</li>
    </ul>
  </li>
  <li>Click Add</li>
  <li>Type domain, then click Check Names</li>
  <li>Click on "Domain Admins", then click OK (x2 times)
    <ul>
      <li>The "Domain Admins" group is a built-in security group. Everyone who’s a member of this group can make changes to the domain (i.e. make new accounts, delete stuff, etc.)</li>
    </ul>
  </li>
  <li>Click Apply, then click OK</li>
</ol>
</p>
<h4>Log out of DC-1, then log back in as the new admin.</h4>
<p>
<ol>
  <li>Open Command Prompt</li>
  <li>Type "logoff" then press enter</li>
  <li>Open the Remote Desktop Connection application on your computer to go back into your DC-1 Remote Desktop Connection.
  <li>Enter credentials (i.e. Username: mydomain.com\jane_admin and Password: Password1)</li>
</ol>
</p>

<h3>Join Client-1 to the Domain.</h3>
<p>
<img src="https://i.imgur.com/T8qkBy6.png" height="80%" width="80%" alt="Client to Domain"/>
</p>
<h4>In Azure, set Client-1's DNS settings to DC-1's NIC Private IP address and then restart Client-1.</h4>
<p>
<ul>
  <li>Earlier, the Virtual Network (Vnet) created a "hidden" DNS server that Client-1 is currently using ("inheriting"). In order to join Client-1 to our domain (mydomain.com), Client-1 needs to use the domain controller (DC-1) as its DNS server. This is because the domain controller (DC-1) knows what our domain (mydomain.com) is.
    <ul>
      <li>If we continue to not use the domain controller (DC-1) as Client-1's DNS server, Client-1 could say "I want to join the domain, mydomain.com" and this "hidden" DNS server created by the Virtual Network (Vnet) would look on the internet for a random "mydomain.com" and fail.
        <ul>
          <li>To confirm, open the Command Prompt, type "ipconfig /all", and press enter to confirm that you are using a public IP address as your DNS server as created by the Virtual Network (Vnet).</li>
        </ul>
      </li>
      <li>If we use the domain controller (DC-1) as Client-1's DNS server, Client-1 could say "I want to join the domain, mydomain.com" and the domain controller (DC-1) would respond "Oh hey, that's me. Use this IP address." since it is acting as Client-1's DNS server.
        <ul>
          <li>After you've finished this section, to confirm, open the Command Prompt, type "ipconfig /all", and press enter to confirm that you are now using the domain controller (DC-1) as your DNS server.</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
</p>
<p>
<ol>
  <li>Search Virtual Machine</li>
  <li>Click DC-1</li>
  <li>Click Networking</li>
  <li>Copy the NIC Private IP</li>
  <li>Search Virtual Machine</li>
  <li>Click Client-1</li>
  <li>Click Networking</li>
  <li>Click on the Network Interface</li>
  <li>Click DNS servers</li>
  <li>Click Custom instead of "Inherit from virtual network"</li>
  <li>Paste DC-1's NIC Private IP</li>
  <li>Click Save</li>
  <li>Search Virtual Machine</li>
  <li>Click Client-1</li>
  <li>Click Restart
    <ul>
      <li>When we restart Client-1 from the Azure Portal, it's going to flush the DNS cache.</li>
    </ul>
  <li>Click Yes</li>
</ol>
</p>
<h4>Using Remote Desktop, log back into Client-1 as the original local admin and join Client-1 to the domain.</h4>
<p>
<ol>
  <li>Log back into your Client-1 Remote Desktop Connection (i.e. Username: labuser and Password: Password1)</li>
  <li>Right click Start (in the bottom right corner of the screen)</li>
  <li>Click System</li>
  <li>Click Rename this PC (advanced)</li>
  <li>Click Change…</li>
  <li>Select "Domain:" and type mydomain.com; Click OK
    <ul>
      <li>We’re typing mydomain.com because remember, we’re pointing to our domain controller which knows what mydomain.com is. This is why a Computer Name/Domain Changes window pops up instead of an error message.</li>
    </ul>
  </li>
  <li>Enter Username (i.e. mydomain.com\jane_admin)</li>
  <li>Enter Password (i.e. Password1)</li>
  <li>Click OK</li>
  <li>Allow the Client-1 Remote Desktop Connection to restart in order to finish joining the domain</li>
    <ul>
      <li>So now, we’re going to be able to actually log into Client-1 with our domain admin (jane_admin) because Client-1 will be a member of the domain controller (DC-1) and the domain admin (jane_admin) will be able to log in from any computers that are on the domain controller (DC-1).</li>
    </ul>
</ol>
</p>

<h3>Configure Remote Desktop for Non-Administrative Users.</h3>
<p>
<img src="https://i.imgur.com/Z5ZhXCa.png" height="80%" width="80%" alt="RD Configuration"/>
</p>
<h4>Using Remote Desktop, log back into Client-1 as the new admin and allow "domain users" access to Remote Desktop.</h4>
<p>
<ul>
  <li>Now we are setting the remote desktop up so that all normal domain users are able to remote into Client-1.
    <ul>
      <li>Right now, only the domain admin (jane_admin) is allowed to remotely log into Client-1.</li>
      <li>We are doing this because eventually, we are going to create a bunch of random domain users inside our domain and then we are going to use one of those random domain users to log into Client-1. This essentially simulates an environment such as a University where you have a lot of students and a lot of computers and any given student can log into any given computer.</li>
      <li>Normally we'd want to make this kind of change with something called "Group Policy" (so you don't need to log into each computer to make changes). However, group policies are a bit out of the scope of this lab.</li>
    </ul>
  </li>
</ul>
</p>
<p>
<ol>
  <li>Log back into your Client-1 Remote Desktop Connection (i.e. Username: mydomain.com\jane_admin and Password: Password1)</li>
  <li>Right click Start (in the bottom right corner of the screen)</li>
  <li>Click System</li>
  <li>Click Remote desktop</li>
  <li>Click Select users that can remotely access this PC
    <ul>
      <li>Notice you can see who is currently allowed access to remote desktop into the computer (i.e. MYDOMAIN\jane_admin)</li>
    </ul>
  </li>
  <li>Click Add</li>
  <li>Type "domain users"; click Check Names; Click OK
    <ul>
      <li>Instead of adding an individual user (because obviously we don't want to add like 10,000 users), we can use this special built-in security group called "domain users" (which all users in the domain are automatically in)</li>
    </ul>
  </li>
  <li>Click OK
    <ul>
      <li>Now, essentially all domain users are allowed to log into this computer. To confirm:
        <ul>
          <li>Go back into your DC-1 Remote Desktop Connection</li>
          <li>In the Service Manager application, click on Tools, then click Active Directory Users and Computers
            <ul>
              <li>Alternatively, in the "Type here to search" section (bottom left corner of the screen), search Active Directory Users and Computers and open search result</li>
              <li>Alternatively, in the "Type here to search" section (bottom left corner of the screen), open the "Windows Administrative Tools" folder then click Active Directory Users and Computers</li>
            </ul>
          </li>
          <li>Click mydomain.com</li>
          <li>Click Users</li>
          <li>Click Domain Users</li>
          <li>Click Members
            <ul>
              <li>Notice all user accounts that are created will automatically get added to this "Domain Users" group because we allowed "Domain Users" to remotely log into Client-1. So anyone who's in this group should be allowed to log into Client-1.</li>
            </ul>
          </li>
          <li>Click Cancel</li>
    </ul>
  </li>
</ul>
</ol>
</p>

<h3>Create and Test Additional User Accounts.</h3>
<p>
<img src="https://i.imgur.com/Qatc4lY.png" height="80%" width="80%" alt="Account Testing"/>
</p>
<h4>Using Remote Desktop, log back into DC-1 as the new admin and create additional users using this <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">code</a> in Powershell.</h4>
<p>
<ul>
  <li>Now we are going to create a bunch of random domain users inside our domain and then we are going to use one of those random domain users to log into Client-1.
    <ul>
      <li>Remember, this essentially simulates an environment such as a University where you have a lot of students and a lot of computers and any given student can log into any given computer</li>
    </ul>
  </li>
</ul>
</p>
<p>
<ol>
  <li>Go back into your DC-1 Remote Desktop Connection (i.e. Username: mydomain.com\jane_admin and Password: Password1)</li>
  <li>Once logged in, in the “Type here to search” section (bottom left corner of the screen), search powershell_ise; right click the search result; click Run as administrator; click Yes
    <ul>
      <li>Powershell is a powerful scripting language native to Windows, offering extensive capabilities for performing a wide range of tasks.</li>
      <li>“ise” stands for “Integrated Scripting Environment”</li>
    </ul>
  </li>
  <li>Open the hyperlink in the instructions above</li>
  <li>Click the two squares icon (“Copy raw contents”)
    <ul>
      <li>You can try to read and understand the code (though you really don’t have to because it is outside the scope of this lab), but essentially it is going to:
        <ul>
          <li>Create 10000 accounts</li>
          <li>All the accounts are going to have Password1 as its password</li>
          <li>The accounts are going to be created and stored in the _EMPLOYEES organizational unit</li>
        </ul>
      </li>
    </ul>
  </li>
  <li>Go back into you DC-1 Remote Desktop Connection in Powershell ISE</li>
  <li>Click the white paper with a yellow badge icon (“New Script”)</li>
  <li>Paste the code</li>
  <li>Click the green play button icon (“Run Script (F5)”)</li>
  <li>Still in your DC-1 Remote Desktop Connection, go back into Active Directory Users and Computers</li>
  <li>Click mydomain.com</li>
  <li>Click  _EMPLOYEES to select, Right click _EMPLOYEES and click Refresh</li>
</ol>
</p>
<h4>Log out of Client-1, then log back into Client-1 with one of the generated user accounts.</h4>
<p>
<ol>
  <li>Still in your DC-1 Remote Desktop Connection, choose any account that was created and stored in the _EMPLOYEES organizational unit and copy the name</li>
  <li>Log out of Client-1</li>
  <li>Log back into Client-1 with the account you just copied (i.e. Username: mydomain.com\[account you just copied] and Password: Password1)
    <ul>
      <li>If you ever want to check which user is logged in, you can open the Command Prompt, type “whoami”, and press enter to confirm</li>
      <li>Side note: You can open “File Explorer”; click “This PC”; click “Windows(C:)”; and then click “Users” to see a folder of every user that has logged into your computer up to that point</li>
    </ul>
  </li>
</ol>
</p>
<h4>Unlock a user account with excessive incorrect password attempts.</h4>
<p>
<ol>
  <li>In your DC-1 Remote Desktop Connection, choose any account that was created and stored in the _EMPLOYEES organizational unit and copy the name</li>
  <li>Attempt to log into Client-1 with the account you just copied (i.e. Username: mydomain.com\[account you just copied], entering the incorrect password ~x10 times
    <ul>
      <li>10 incorrect password attempts should lock the account you are trying to log into</li>
    </ul>
  </li>
  <li>Go back into your DC-1 Remote Desktop Connection</li>
  <li>Right click the name of the account you copied earlier</li>
  <li>Click Properties</li>
  <li>Click Account</li>
  <li>Click the “Unlock account” box to check it</li>
  <li>Click OK</li>
</ol>
</p>
<h4>Reset a user account's password.</h4>
<p>
<ol>
  <li>In your DC-1 Remote Desktop Connection, right click the account of the person in the _EMPLOYEES organizational unit that forgot their password (select any account for the sake of this exercise)</li>
  <li>Click Reset Password</li>
  <li>Reset their password to something new by entering a new password and confirming
    <ul>
      <li>Side note: Notice you can “Unlock the user’s account” here too</li>
    </ul>
  </li>
  <li>Click OK (x2 times)</li>
</ol>
</p>
<h4>Disable and enable a user account.</h4>
<p>
<ol>
  <li>In your DC-1 Remote Desktop Connection, right click the account of the person in the _EMPLOYEES organizational unit that you want to disable</li>
  <li>Click Disable Account</li>
  <li>Click OK
    <ul>
      <li>Now you can try to log into Client-1 with the account you disabled (i.e. Username: mydomain.com\[account you just disabled] and Password: Password1) and you should run into the following message: “The user account is currently disabled and cannot be used. For assistance, contact your system administrator or technical support.”</li>
    </ul>
  </li>
  <li>In your DC-1 Remote Desktop Connection, right click the account of the person in the _EMPLOYEES organizational unit that you want to re-enable</li>
  <li>Click Enable Account</li>
  <li>Click OK
    <ul>
      <li>Now you should be able to log into Client-1 with the account you re-enabled (i.e. Username: mydomain.com\[account you just re-enabled] and Password: Password1)</li>
    </ul>
  </li>
</ol>
</p>

<h3>Delete the Resource Groups created in this lab to avoid incurring costs and verify its deletion.</h3>
<p>
<img src="https://i.imgur.com/T5lFp7W.png" height="80%" width="80%" alt="Delete RG and Verify"/>
</p>
<p>
<ol>
  <li>Search for Resource Group</li>
  <li>Click on your resource group</li>
  <li>Click Delete resource group</li>
  <li>Type or copy/paste the name of your resource group to confirm deletion
    <ul>
      <li>Repeat these steps for every resource group created in this lab</li>
    </ul>
  </li>
  <li>Search for Resource Group</li>
  <li>Confirm that your resource group has been deleted</li>
</ol>
</p>
