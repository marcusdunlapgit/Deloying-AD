<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Deploying On-premises Active Directory in Azure</title>
</head>
<body>

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation and configuration of on-premises Active Directory within Azure Virtual Machines.<br/>

<h2>Video Demonstration</h2>
<ul>
  <li><strong>[YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)</strong></li>
</ul>

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Remote Desktop</li>
  <li>Active Directory Domain Services</li>
  <li>PowerShell</li>
</ul>

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 (21H2)</li>
</ul>

<h2>High-Level Deployment and Configuration Steps</h2>
<ul>
  <li>Provision Azure VMs and Network</li>
  <li>Install and Configure AD DS</li>
  <li>Join Client to Domain</li>
  <li>Create and Test Domain Users</li>
</ul>

<h2>Deployment and Configuration Steps</h2>

<!-- PART 1 -->
<h3>🛠️ Part 1 - Initial Setup and Domain Configuration</h3>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Deployment Steps"/>
</p>
<p>
<strong>Step 1: Provision Resources in Azure</strong><br/>
- Create a Resource Group: <code>Active-Directory-RG</code><br/>
- Create a Virtual Network: <code>Active-Directory-VNET</code><br/>
- Place both in the same region.
</p>

<p>
<strong>Step 2: Setup DC-1 (Domain Controller)</strong><br/>
- Create a Windows Server 2022 VM named <code>DC-1</code><br/>
- Username: <code>labuser</code> | Password: <code>Cyberlab123!</code><br/>
- Assign a static private IP<br/>
- Disable Windows Firewall via <code>wf.msc</code> for testing<br/>
</p>

<p>
<strong>Step 3: Setup Client-1</strong><br/>
- Create a Windows 10 VM named <code>Client-1</code><br/>
- Attach to same VNET and region as DC-1<br/>
- Set Client-1 DNS to DC-1 private IP<br/>
- Restart VM, ping DC-1 to confirm connectivity<br/>
- Run <code>ipconfig /all</code> to verify DNS points to DC-1<br/>
</p>

<p>
<strong>Step 4: Install and Promote Active Directory</strong><br/>
- On DC-1: Open Server Manager → Add Roles and Features → Active Directory Domain Services<br/>
- Promote DC-1 to domain controller → New forest: <code>mydomain.com</code><br/>
- Use password <code>Password1</code> during setup<br/>
- Restart and log in as: <code>mydomain.com\labuser</code><br/>
</p>

<p>
<strong>Step 5: Create Admin User</strong><br/>
- In ADUC: Create OUs: <code>_EMPLOYEES</code> and <code>_ADMINS</code><br/>
- Create user <code>jane_admin</code> (Jane Doe), password: <code>Cyberlab123!</code><br/>
- Add to <code>Domain Admins</code> group<br/>
- Log in as <code>mydomain.com\jane_admin</code> and use this from now on<br/>
</p>

<p>
<strong>Step 6: Join Client-1 to Domain</strong><br/>
- Log into Client-1 as <code>labuser</code>, join to <code>mydomain.com</code><br/>
- Restart the machine and log in to DC-1<br/>
- In ADUC: verify Client-1 appears, drag it into <code>_CLIENTS</code> OU<br/>
</p>

<br/>

<!-- PART 2 -->
<h3>🧑‍💼 Part 2 - Remote Desktop and Domain User Access</h3>

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Deployment Steps"/>
</p>
<p>
<strong>Step 1: Allow RDP for Domain Users</strong><br/>
- Log into Client-1 as <code>mydomain.com\jane_admin</code><br/>
- Open System Properties → Remote Desktop<br/>
- Add <code>Domain Users</code> to allowed RDP users<br/>
</p>

<p>
<strong>Step 2: Create Additional Domain Users</strong><br/>
- Log into DC-1 as <code>jane_admin</code><br/>
- Open PowerShell ISE (Admin) and run a script to bulk-create users<br/>
- Verify users in <code>_EMPLOYEES</code> OU in ADUC<br/>
</p>

<p>
<strong>Step 3: Test Logins</strong><br/>
- Attempt RDP login to Client-1 using one of the new users<br/>
- Confirm that user can log in without administrative rights<br/>
</p>

<p><strong>⚠️ Reminder:</strong> Do not delete the VMs after the lab. If finished, stop them from the Azure Portal to save costs.</p>

</body>
</html>
