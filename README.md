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

<h2>Steps</h2>



**1. Prepare Your Azure Environment**

1. Create an Azure Account: Sign up for a free or paid Azure subscription if you don’t already have one. Please check my tutorial on how to create a VM with a resource group on Microsoft Azure: https://github.com/josephaadams/azure-vmcreation

2. Set Up a Resource Group: Navigate to the Azure portal and create a new resource group to manage your resources efficiently.

3. Deploy Virtual Network (VNet): Create a virtual network with at least two subnets:

- Subnet 1: For the Active Directory Domain Controllers (DCs).

- Subnet 2: For clients or other servers in the domain.


**2. Deploy Virtual Machines (VMs)**

1. Create Virtual Machines:

- DC-1: For the first Domain Controller. For the image choose Windows Server 2022.

- Client-1: For client machine testing. For the image choose Windows 10 Pro.

- Size: Choose a minimum of 2vcpus.

2. Assign Static Private IPs:

- Configure static private IP addresses for the VMs to ensure consistent connectivity.

<img src="https://i.imgur.com/zHMw1jx.png"/>
<img src="https://i.imgur.com/DaRp0Ai.png"/>



**3. Install and Configure Active Directory**


1. Install Active Directory Domain Services (AD DS):

- Log in to DC-1 using Remote Desktop.

- Open Server Manager.

- Click on Manage > Add Roles and Features.

- Select Role-based installation.

- Choose the DC-1 server and click Next.

- Under Server Roles, check Active Directory Domain Services, then click Add Features when prompted.

- Click Next, then Install, and wait for the installation to complete


<img src="https://i.imgur.com/TvT0yXh.png"/>
<img src="https://i.imgur.com/BsIs29m.png"/>
<img src="https://i.imgur.com/r9ZAaIp.png"/>


  

2. Promote the Server to a Domain Controller:

- After installation, go back to Server Manager.

- Click on the flag notification at the top and select Promote this server to a domain controller.

- Choose Add a new forest and enter the domain name (e.g., mydomain.com).

- Set up the Forest Functional Level (choose the highest available version).

- Create and confirm a Directory Services Restore Mode (DSRM) password.

- Click Next through the additional options, ensuring DNS Server is selected.

- Review and Install, then restart the server once the promotion is complete.

- Log back in using mydomain.com\labuser.

  

3. Create a Domain Admin User:

- Open Active Directory Users and Computers (ADUC) from Server Manager > Tools.

- Expand your domain (mydomain.com).

- Right-click on the domain name, go to New > Organizational Unit, and name it _EMPLOYEES.

- Repeat the process to create another OU named _ADMINS.

- Right-click _ADMINS, select New > User.

- Enter First Name: Laura, Last Name: Obando, User logon name: laura_admin, then click Next.

- Set the password to Password1! and ensure Password never expires is checked.
  
- Click Next and Finish.

- Right-click laura_admin, select Properties.

- Under Member Of, click Add, type Domain Admins, then click OK.

- Log out and log back in as mydomain.com\laura_admin.


<img src="https://i.imgur.com/SVKvyy1.png"/>
<img src="https://i.imgur.com/noFrwwm.png"/>
<img src="https://i.imgur.com/jVvadHw.png"/>


4. Install DNS Services:

- DNS is installed automatically with AD DS, but you should verify it.

- Open DNS Manager from Server Manager > Tools.

- Expand the server name and ensure that your domain name appears under Forward Lookup Zones.

- Right-click the domain name, go to Properties, and ensure Dynamic updates is set to Secure only.

- Use nslookup on a command prompt to test DNS resolution for mydomain.com.


<img src="https://i.imgur.com/XAM3FK0.png"/>


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


<img src="https://i.imgur.com/m8pri8l.png"/>

**6. Bulk Create Users**

1. Log in to DC-1 as jane_admin.

2. Open PowerShell_ise as an administrator.

3. Use a PowerShell script to bulk create users in the _EMPLOYEES OU.

4. Test logging into Client-1 with one of the newly created accounts.

<img src="https://i.imgur.com/lMPKPUa.png"/>



