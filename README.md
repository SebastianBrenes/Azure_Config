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

- Promote the server to a domain controller and create a new forest (e.g., `myadproject.com`).
<img src="" alt="">

- Restart the server and log in using the domain credentials.
<img src="" alt="">

### Step 4: Manage Active Directory Users and Computers
- Open Active Directory Users and Computers (ADUC).
<img src="" alt="">

- Create OUs for employees (`_EMPLOYEES`) and admins (`_ADMINS`).
<img src="" alt="">

- Add users (e.g., `jane_admin`) and assign them to the appropriate security groups.
<img src="" alt="">

- Join `User-1` to the domain and move it into the `_USERS` OU.
<img src="" alt="">

### Step 5: Set Up Remote Desktop for Users
- On `User-1`, allow "Domain Users" to access Remote Desktop.
<img src="" alt="">

- Test the connection with a non-administrative user account.
<img src="" alt="">

### Step 6: Create and Test Additional User Accounts
- Use a PowerShell script to bulk-create user accounts.
<img src="" alt="">

- Verify the accounts in ADUC and test logging into `User-1` with one of the new accounts.
<img src="" alt="">
