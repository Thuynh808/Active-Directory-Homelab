# Active Directory Homelab Project

![Active Directory Homelab](https://i.imgur.com/rDYFHff.png)
<br><br>
![Active Directory Homelab2](https://i.imgur.com/hGfReqr.png)


## Introduction

Welcome to my dynamic Homelab project, where I'll be crafting a virtual environment to simulate a mini corporate setup. Through VirtualBox, I've orchestrated two key players: a Windows Server 2019 Domain Controller and a Windows 10 Client. The Domain Controller stands as the authority figure, managing the digital domain, while the Windows 10 Client plays the role of an employee navigating within this intricate network. As I dive into configuring network interfaces, my aim is to create a functional small-scale representation that reflects the complexities of a real-world corporate IT environment. Join me as I navigate the details of this Homelab journey, aiming to create a rich learning experience in network management and system administration.


The Active Directory Homelab architecture includes:


- **VirtualBox:** The foundation for creating and managing virtual machines, facilitating real-world scenario simulation

- **Windows Server 2019:** As the Domain Controller, it manages users, devices, and policies, forming the core of Active Directory Domain Services

- **Windows 10:** Representing an employee workstation, it provides insights into the corporate end-user experience

- **Network Interfaces:** Enabling communication, with one for internet access and another for internal network interaction

- **Active Directory Domain Services:** Crucial for user and network organization, structuring users, computers, and resources coherently

- **DHCP:** Automates IP address assignment, streamlining network configuration

- **RAS/NAT:** Facilitates seamless internal-external network connectivity

- **User List Text File:** Stores user info, aiding efficient user setup and management

- **Python Script:** Adds automation and customization, enhancing user creation

- **Group Policy Object:** Enforces network-wide policies, maintaining settings, security, and access controls


Collectively, these tools and architectural elements culminate in an environment where virtual machines interact harmoniously, mirroring the intricacies of a corporate ecosystem. Through exploration and manipulation, this lab offers an immersive platform to experiment, learn, and finesse the art of network management and system administration.

<details>
  <summary><h2><b>Section 1: NETWORK INTERFACE CARD (NIC) CONFIGURATIONS</b></h2></summary>
  <br> <br>
  In this section, We'll be configuring the 2 NICs on the Windows Server 2019.<br><br>
  
  ![Image 1](https://i.imgur.com/wDilWI5.png)
  <br><br>
  
  **Step 1: Access Network Settings:**
  - Open "Network Connections" from the Control Panel
  
  **Step 2: Identify NICs:**
  - Identify the two NICs and renaming them to "Internet" and "Internal"
  
  **Step 3: Assign IP Addresses and Configure DNS:**
  - For NIC 1 (Internal):
    - IP Address: 10.2.22.1
    - Subnet Mask: 255.255.255.0
    - Default Gateway: (empty)
    - Preferred DNS Server: 127.0.0.1
  - For NIC 2 (Internet):
    - Obtain IP settings automatically (DHCP) for internet access
    - Obtain DNS server address automatically 
  
  **Reasons for the Configuration:**
  - NIC 1: Provides a gateway for the internal network.
    - **Explanation:** NIC 1 with IP "10.2.22.1" connects devices inside our network. We don't set a gateway to keep this network separate from the internet.
  - NIC 2: Enables connection to the internet.
    - **Explanation:** NIC 2 gets settings from the network, letting us connect online easily.
   
</details>

<details>
  <summary><h2><b>Section 2: INSTALL ACTIVE DIRECTORY DOMAIN SERVICES</b></h2></summary>
  <br><br>
  
  In this section, we'll be installing Active Directory Domain Services (AD DS) on Windows Server 2019.<br><br>
  
  
  **Step 1: Install AD DS:**
  - Open Server Manager.
  - Click "Manage" > "Add Roles and Features."
  - Choose "Role-based or feature-based installation" and click "Next."
  - Select the local server and click "Next."
  - Check "Active Directory Domain Services" and proceed.
  - Click through until you reach the installation summary, then click "Install."<br><br>

  ![Image 2](https://i.imgur.com/L2OqS5J.png)
<br><br>
  
  **Step 2: Promote Server to Domain Controller:**
  - After installation, click "Promote this server to a domain controller."
  - Choose "Add a new forest" and set domain details.
    - Server: DC
    - Operation System: Windows Server 2019
    - Domain Name: Streetrack.com
  - Set a Directory Services Restore Mode (DSRM) password.
  - DNS can be left alone for automatic configuration.
  - Complete the wizard and let the server restart.<br><br>
  
  ![Image 3](https://i.imgur.com/2TLFD6o.png)
<br><br>
  
  Awesome! We've successfully installed and configured Active Directory Domain Services on our Windows Server 2019.
</details>

<details>
  <summary><h2><b>Section 3: CREATING USERS AND ORGANIZATIONAL UNITS (OUs)</b></h2></summary>
  <br><br>
  
  Here, we'll be exploring how to efficiently manage users by creating Organizational Units (OUs), adding users, and assigning administrative privileges.<br><br>
  
  ![Image 4](https://i.imgur.com/eGgqXno.png)
<br><br>
  
  **Step 1: Create Organizational Units (OUs):**
  - Open "Active Directory Users and Computers."
  - Right-click on the domain name and choose "New" > "Organizational Unit."
  - Name the OU "_ADMINS" and "_USERS" respectively.<br><br>

  ![Image 5](https://i.imgur.com/nBDdKb0.png)
<br><br>
  
  **Step 2: Create User Account:**
  - Right-click on the "_ADMINS" OU and choose "New" > "User."
  - Enter user details:
    - First Name: Thong
    - Last Name: Huynh
    - User Logon Name: thuynh<br><br>
      
  ![Image 6](https://i.imgur.com/kozipGr.png)
<br><br>

  **Step 3: Add User to Domain Admins Group:**
  - Locate the user we just created and right-click.
  - Select "Properties."
  - In the "Member Of" tab, click "Add."
  - Enter "Domain Admins" and click "Check Names."
  - Click "OK" to add the user to the "Domain Admins" group.<br><br>
  
  ![Image 7](https://i.imgur.com/TA0MoBV.png)<br><br>
  
  ![Image 8](https://i.imgur.com/bgI8oMM.png)<br><br>
  
  **Step 4: Verify User and OU Creation:**
  - Refresh Active Directory by restarting and log in with new Admin User credentials to confirm User and OU Creation.<br><br>
  
  ![Image 9](https://i.imgur.com/DLMH7ra.png)
<br><br>
  
  Yay! we've successfully created Organizational Units (OUs), added a user to the "_ADMINS" OU, and granted administrative privileges by adding our user to the "Domain Admins" group.
</details>

<details>
  <summary><h2><b>Section 4: INSTALL & CONFIGURE DHCP</b></h2></summary>
  <br><br>
  
  In this section, we'll explore the process of installing and configuring the Dynamic Host Configuration Protocol (DHCP) to automate IP address assignment within our network.
  
  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019.<br><br>
  
  ![Image 10](https://i.imgur.com/0yCKtnX.png)<br><br>
  
  **Step 2: Add DHCP Role:**
  - Click "Manage" > "Add Roles and Features."
  - Select "Role-based or feature-based installation" and click "Next."
  - Choose the local server(DC) and proceed.
  - Check "DHCP Server" and complete the installation wizard.<br><br>
  
  ![Image 11](https://i.imgur.com/HUQbLnu.png)<br><br>

  **Step 3: Configure DHCP:**
  - After installation, open "DHCP Manager" from "Administrative Tools."
  - Right-click on our server name and choose "Configure DHCP."
  - Follow the wizard, selecting the appropriate network connection.<br><br>
  
  ![Image 12](https://i.imgur.com/A4WWdGF.png)<br><br>
  
  **Step 4: Create DHCP Scope:**
  - In "DHCP Manager," right-click on "IPv4" and choose "New Scope."
  - Set the scope name, IP range, subnet mask, default gateway, DNS servers, and lease duration:
    - Scope Name: 10.2.22.100-200
    - Start IP Address: 10.2.22.100
    - End IP Address: 10.2.22.200
    - Length: 24
    - Subnet Mask: 255.255.255.0
    - Default Gateway: 10.2.22.1
    - DNS: 127.0.0.1
    - Lease Duration: 8 days<br><br>
  
  ![Image 13](https://i.imgur.com/5n4O0CU.png)<br><br>

  **Step 5: Authorize DHCP Server:**
  - If needed, we'll right-click on the server name in "DHCP Manager" and choose "Authorize."<br><br>
  
  Great! We've successfully installed and configured DHCP, automating IP address assignment to devices within our network.
</details>

<details>
  <summary><h2><b>Section 5: INSTALL & CONFIGURE NETWORK ADDRESS TRANSLATION (NAT)</b></h2></summary>
  <br><br>
  
  In this section, we'll focus on installing and configuring Network Address Translation (NAT), a technique that enables devices within our internal network to access the external internet while using a single public IP address.<br><br>
  
  ![Image 5](images/image5.jpg)
  
  **Step 1: Open Server Manager:**
  - Launch "Server Manager" on the Windows Server 2019
  
  **Step 2: Configure NAT:**
  - After installation, open "Routing and Remote Access" from "Administrative Tools"
  - Right-click on our server name and choose "Configure and Enable Routing and Remote Access"
  - Follow the wizard, selecting "Network address translation (NAT)"
  - Select the external network interface(Internet) for public connection
  
  **Step 3: Enable NAT and Join Domain:**
  - In the "NAT" section, right-click on the server name and choose "NAT" > "Enable."
  - On the client VM (Windows 10), log in using the domain admin "thuynh"
  - Join the domain using these steps:
    - Open "System Properties" on the client VM
    - Go to the "Computer Name" tab and click "Change"
    - Choose "Domain" and enter our domain name "Streetrack.com"
    - Provide the "thuynh" credentials to join the domain.
  
  **Step 4: Test Connectivity:**
  - On the client VM, open a Command Prompt.
  - Use the following commands to verify network settings and connectivity:
    - Run `ipconfig` to check the assigned IP configuration.
    - Run `ping google.com` to test internet connectivity.
  
  YES! We've successfully configured Network Address Translation (NAT), joined the domain using "thuynh" credentials, and verified internet and internal network connectivity on the client VM.
</details>


<details>
  <summary>Section 6: CREATE AND RUN PYTHON SCRIPT</summary>
  
  Describe the architecture components you mentioned earlier in this section.

  ![Image 2](images/image2.jpg)
</details>

<details>
  <summary>Section 7: GROUP POLICY CONFIGURATIONS</summary>
  
  Describe the architecture components you mentioned earlier in this section.

  ![Image 2](images/image2.jpg)
</details>

<details>
  <summary>Section 8: CONFIRM FUNCTIONALITY </summary>
  
  Describe the process of setting up the Windows Server's internal NIC here.

  ![Image 3](images/image3.jpg)
</details>



