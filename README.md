<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. By following these steps, you'll learn how to set up a cloud-based environment that simulates a traditional on-premises Active Directory infrastructure.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>



**1. Prepare Your Azure Environment**

1. Create an Azure Account: Sign up for a free or paid Azure subscription if you don’t already have one.

2. Set Up a Resource Group: Navigate to the Azure portal and create a new resource group to manage your resources efficiently.

3. Deploy Virtual Network (VNet): Create a virtual network with at least two subnets:

- Subnet 1: For the Active Directory Domain Controllers (DCs).

- Subnet 2: For clients or other servers in the domain.


**2. Deploy Virtual Machines (VMs)**

1. Create Virtual Machines:

- DC-1: For the first Domain Controller.

- Client-1: For client machine testing.

- Choose a Windows Server operating system (e.g., Windows Server 2022).

2. Assign Static Private IPs:

- Configure static private IP addresses for the VMs to ensure consistent connectivity.


**3. Install and Configure Active Directory**


1. Install Active Directory Domain Services (AD DS):

- Log in to DC-1.

- Use the Server Manager to install the "Active Directory Domain Services" role.

2. Promote the Server to a Domain Controller:

- Configure the domain name (e.g., mydomain.com).

- Set up a new forest.

- Restart and log back in to DC-1 as mydomain.com\labuser.

3. Create a Domain Admin User:

- Open Active Directory Users and Computers (ADUC).

- Create an Organizational Unit (OU) called _EMPLOYEES.

- Create a new OU named _ADMINS.

- Create a new user named jane_admin with password Cyberlab123! and add it to the Domain Admins Security Group.

- Log out and log back in as mydomain.com\jane_admin.

4. Install DNS Services:

- DNS services are installed automatically with AD DS. Verify configuration to ensure domain name resolution works correctly.


**4. Join Client Machines to the Domain**


1. Deploy Client VM:

- Create Client-1 within the same VNet.

- Ensure it uses the DNS server IP address of DC-1.

2. Join Client-1 to the Domain:

- From the Azure portal, set Client-1’s DNS settings to DC-1’s private IP address.

- Restart Client-1.

- Log in to Client-1 as labuser (local admin) and join it to mydomain.com.

- Verify that Client-1 appears in ADUC under _CLIENTS (create the _CLIENTS OU if it doesn’t exist and move Client-1 there).

3. Verify Domain Functionality:

- Test user logins and group policy applications.


**5. Enable Remote Desktop For Users**

1. Enable Remote Desktop for Non-Administrative Users

2. Log in to Client-1 as mydomain.com\jane_admin.

3. Open System Properties and click "Remote Desktop."

4. Allow "Domain Users" access to Remote Desktop.

5. Test logging into Client-1 as a non-administrative user.



**6. Bulk Create Users**

1. Log in to DC-1 as jane_admin.

2. Open PowerShell_ise as an administrator.

3. Use a PowerShell script to bulk create users in the _EMPLOYEES OU.

4. Test logging into Client-1 with one of the newly created accounts.



