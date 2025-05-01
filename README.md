# azure-domain-controller
Guide to deploying a Domain Controller in Azure and integrating with Microsoft Entra ID

### Table of Contents

Part 1: Deploy Domain Controller VM

Part 2: Promote to Domain Controller & Configure DNS

Part 3: Create AD Users and Groups

Part 4: Prepare for Microsoft Entra Connect

Part 5: Install and Configure Entra Connect

Post-Installation Tasks

Understanding Microsoft Entra Sign-In Methods


### Part 1: Deploy Domain Controller VM

1. Create the VM

Name: DomainController-vm

OS: Windows Server 2016/2019/2022

SKU: Standard B2s or D2s_v3

Network: Attach to a custom VNet

2. Assign Static Private IP

Navigate to: DomainController-vm > Networking > Network Interface > IP Configurations > ipconfig1

Set Private IP to Static (e.g., 10.0.0.4)

![image](https://github.com/user-attachments/assets/a96d3b20-448f-49b5-99f7-ebd934a690e1)


3. Configure DNS on VNet

Go to your Virtual Network > DNS Servers

Set to Custom and enter the static IP: 10.0.0.4

Save and restart the VM

### Part 2:  Promote to Domain Controller & Configure DNS

1. RDP into the VM

2. Install Roles

Open Server Manager

Add AD DS and DNS Server roles

3. Promote to Domain Controller

Choose: Add a new forest

Set DSRM password (e.g., Password@123)

Complete the wizard (ignore benign warnings)

4. Configure DNS Forwarding

Server Manager > Tools > DNS > Right-click Server > Properties > Forwarders

Set DNS Forwarder: 8.8.8.8

5. Verify DNS with ipconfig /all

Confirm the DNS is set to 10.0.0.4

### Part 3: Create AD Users and Groups

Open Active Directory Users and Computers

Navigate to YourDomain > Users

Create necessary users and security groups


### Part 4: Prepare for Microsoft Entra Connect

1. In Azure

Open Microsoft Entra ID

Create a Global Administrator account

2. On the Domain Controller

RDP into DomainController-vm

Open Server Manager > Local Server

3. Optional

Create any additional users/groups in AD before sync


 ### Part 5: Install and Configure Entra Connect

1. Download

Get AzureADConnect.msi from Microsoft

(Temporarily disable Enhanced Security if needed)

2. Express Setup

Choose: Use Express Settings

3. Connect

Sign in with Global Admin (Microsoft Entra ID)

Sign in with Domain Admin (on-prem)

4. Configure Sync

Review and complete the setup wizard

 ### Post-Installation Tasks

1. Sign Out and In

Re-login to activate sync tools

![image](https://github.com/user-attachments/assets/fe21446d-e588-4da1-8cc6-5aefd8e49390)


2. Sync Frequency

Default: Every 30 minutes

Can be customized via PowerShell

Optional: Enable Password Writeback

Requires Microsoft Entra ID Premium

Re-open Entra Connect > Configure > Customize Synchronization Options

![image](https://github.com/user-attachments/assets/11610d0b-426f-4b0b-adc1-896f1ff6924a)

### Validation & Testing

Try logging into https://portal.azure.com using a synced user.

Verify sign-in and role assignments.

