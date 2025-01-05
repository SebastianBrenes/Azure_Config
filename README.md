<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# Configuring and Managing Azure Active Directory in Virtual Machines

This guide walks you through setting up and managing an on-premises Active Directory within Azure Virtual Machines. It's designed to give you a clear understanding of the process while keeping the steps straightforward and practical.

---

## Environments and Tools Used

- **Platforms**: Microsoft Azure (Virtual Machines), Remote Desktop
- **Technologies**: Active Directory Domain Services, PowerShell
- **Operating Systems**: Windows Server 2022, Windows 10 (21H2)

---

## Overview of Steps

1. **Create and Set Up Azure Resources**
   - Set up a domain controller VM (Windows Server 2022) and a client VM (Windows 10).
   - Ensure both VMs are on the same Virtual Network (Vnet).
   - Assign a static private IP to the domain controller.

2. **Enable Connectivity**
   - Verify communication between the client and the domain controller using ping.
   - Adjust firewall settings to allow ICMP traffic.

3. **Install and Configure Active Directory**
   - Install Active Directory Domain Services on the domain controller.
   - Promote the server to a domain controller and set up a new forest.

4. **Manage Users and Computers in Active Directory**
   - Create organizational units (OUs) for employees and administrators.
   - Add admin and standard user accounts.
   - Join the client VM to the domain.

5. **Set Up Remote Desktop for Users**
   - Allow non-administrative users to access the client VM via Remote Desktop.

6. **Create Additional Users**
   - Use a PowerShell script to bulk-create users and test logins.

7. **Clean Up Resources**
   - Remove all Azure resources to avoid unnecessary charges.

---

## Detailed Steps

### Step 1: Create and Configure Azure Resources
- Set up a domain controller VM named `DC-1` with Windows Server 2022.
<img src="https://i.imgur.com/2Wn7spd.png" alt="Create DC-1 VM"/>

- Create a client VM named `User-1` using the same resource group and Virtual Network.
<img src="https://i.imgur.com/dhAuo5O.png" alt="Create User-1 VM">

- Assign a static private IP address to the domain controller (`DC-1`) and verify connectivity using Azure's Network Watcher.
<img src="https://i.imgur.com/SSmlDvD.png" alt="Assing static private IP to DC-1">
<img src="https://i.imgur.com/rlioW0G.png" alt="Using the Azure's Network Watcher">

### Step 2: Ensure Connectivity
- From `User-1`, use `ping -t` to check the connection to `DC-1`.
<img src="https://i.imgur.com/piArwbf.png" alt="Trying to ping to the DC-1">

- On `DC-1`, enable ICMPv4 traffic through the Windows Firewall.
<img src="https://i.imgur.com/TXNIW9G.png" alt="Enable ICMPv4 on the DC-1 machine">

- Verify successful pings from `User-1`.
<img src="https://i.imgur.com/d1mSzRb.png" alt="Succesful Ping from User-1">

### Step 3: Install Active Directory
- Log in to `DC-1` and install Active Directory Domain Services.
<img src="https://i.imgur.com/m8ccDnF.png" alt="Install AD Domain Services">

- Promote the server to a domain controller and create a new forest (e.g. `adlab.com`).
<img src="https://i.imgur.com/jkGnPP2.png" alt="Promote the server">

- Restart the server and log in using the domain credentials.

### Step 4: Manage Active Directory Users and Computers
- Open Active Directory Users and Computers (ADUC).
<img src="https://i.imgur.com/H5Dm99M.png" alt="Open ADUC">

- Create OUs for employees (`_EMPLOYEES`) and admins (`_ADMINS`).
<img src="https://i.imgur.com/L2xMPmY.png?1" alt="Create OU option">
<img src="https://i.imgur.com/yeOWl74.png" alt="Create _EMPLOYEES OU">

- Add users (e.g., `rreynolds_admin`) and assign them to the appropriate security groups.
<img src="https://i.imgur.com/Rrl87np.png" alt="Create new user">
<img src="https://i.imgur.com/Qe0ba9j.png" alt="Assign user to the Domain Admins group">

- Join to the `User-1` VM and using the `rreynolds_admin` account.
We'll set up the dns of the server on the User-1 VM.
<img src="https://i.imgur.com/T4wxMBk.png" alt="Set-up DNS on User-1">
<img src="https://i.imgur.com/deMxtZo.png" alt="Join User-1 through RDP">

- On `User-1`, we are going to allow "Domain Users" to access Remote Desktop.
<img src="https://i.imgur.com/5DYC074.png" alt="Allow Domain Users to access RD">

### Step 5: Create and Test Additional User Accounts
- Using a PowerShell script, we are going to bulk-create a 1000 user accounts.
- We'll need to open Powershell ISE as administrator.
<img src="https://i.imgur.com/aUSHKqi.png" alt="Open Powershell ISE">

- Create a new script and paste the content of this file: [https://github.com/Xinloiazn/configure-ad/blob/main/adscript.ps1](https://github.com/SebastianBrenes/Azure_Config/blob/main/adscript.ps1)
<img src="https://i.imgur.com/t9xmQly.png" alt="Paste new script">

- Run the script and see the new users you're creating.
<img src="https://i.imgur.com/g8xmZnH.png" alt="Run script">


- Verify the accounts in ADUC and test logging into `User-1` with one of the new accounts.
<img src="https://i.imgur.com/8pGMLco.png" alt="Verify the accounts created correctly">
<img src="https://i.imgur.com/SHWyTxY.png" alt="Logging into one of those accounts">
