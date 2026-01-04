# azure-active-directory-lab
# Active Directory in Azure Lab

This project demonstrates how to deploy a fully functional Windows Active Directory environment inside Microsoft Azure using virtual machines, virtual networking, and DNS configuration.  
It simulates an on-premises domain controller and client workstation in a cloud-hosted environment — a common setup for cybersecurity, system administration, and enterprise IT training.
---
## 🚀 Project Overview
This lab sets up:
- **Resource Group**
- **Azure Virtual Network (VNet)**
- **Windows Server 2022 Domain Controller (DC-1)**
- **Windows 10/11 Client Machine (CLIENT-1)**
- **DNS configuration for domain joining**
- **Connectivity verification between machines**
- **Active Directory Domain Services (AD DS)** installation and configuration

The goal is to gain hands-on experience building and managing an Active Directory environment in the cloud.
 
---
# 🧩 Architecture Diagram
![Active Directory Lab Diagram](architecture.svg)
---
## 🏗️ Environment Architecture
### High Level Design
Azure environment contains:
#### 1. Azure Subscription
- Hosts all deployed cloud resources
- Provides billing, IAM, and cloud governance
#### 2. Resource Group: azure-lab
  - Logical container for all AD lab resources
#### 3. Virtual Network: AD_VNet (10.0.0.0/24)
- Provides network connectibity between VMs
- Includes single subnet: default (10.0.0.0/25)
#### 4. Domain Controller: DC-1
  - Provides network connectivity between VMs
  - Includes a single subnet: default (10.0.0.0/25)
#### 5. Client Machine: CLIENT-1
- Windows 10/11 Pro
- Private IP: 10.0.0.5
- DNS configured to use DC-1 (10.0.0.4)
- Joined to domain: lab.local
---
# Step-by-Step Deployment Guide
## 🥇 A — Deploy Azure Infrastructure
### 1. Create a Resource Group
Azure Portal → Resource Groups → Create
- Name: azure-lab
- Region: (choose your closest)
#### Resource Group Creation

  <img width="810" height="735" alt="azure-lab1" src="https://github.com/user-attachments/assets/db2321bd-2fa6-49d8-a3d4-3e7cec86cef8" />
  
  ---
  
  ### 2. Create the Virtual Network
  Azure Portal → Virtual Networks → Create
  - Name: AD_VNet
  - Address Space: 10.0.0.0/24
  - Subnet:
    -   Name: default
    -   Range: 10.0.0.0/25  
### VNet + Subnet Configuration
##### Basic Setup
<img width="1042" height="777" alt="AD-VNET" src="https://github.com/user-attachments/assets/23eb2ce4-e0b0-4c8e-a8b7-15c10aa3d999" />

##### IP addresses/subnet Setup
<img width="983" height="645" alt="AD-VNET-IP" src="https://github.com/user-attachments/assets/d9b8e9a8-a310-4ec6-a89c-d402893ff40c" />

---
## 🥈 B — Deploy the Virtual Machines
### 1. Deploy the Domain Controller (DC-1)
Azure Portal → Virtual Machines → Create VM
### Configuration:
- Image: Windows Server 2022 Datacenter
- Size: Standard B2s (or similar)
- VNet: AD_VNet
- Subnet: default
- Private IP: Static → assign 10.0.0.4
- NIC DNS Settings → Custom DNS: 10.0.0.4 (self)
#### DC-1 Networking / Static IP
##### DC-1 Basic Setup
<img width="1207" height="792" alt="DC-1_basic" src="https://github.com/user-attachments/assets/56d2051e-8be6-4b93-8bb0-74a3ea1f9db2" />

##### Networking
<img width="925" height="665" alt="DC-1_networking" src="https://github.com/user-attachments/assets/2616dc4f-232b-4d35-bdd2-673814e13915" />

##### IP Static
<img width="1910" height="868" alt="DC-1_IP_Static" src="https://github.com/user-attachments/assets/3cc8aaab-996c-4277-af90-acc4da86e671" />

---
### 2. Deploy the Client Machine (CLIENT-1)
### Configuration:
- Image: Windows 10/11 Pro
- Private IP: Dynamic (will receive 10.0.0.5)
- DNS Server: Manually set to 10.0.0.4
#### CLIENT-1 DNS Settings
##### CLIENT-1 Basic Setup
<img width="1118" height="765" alt="CLIENT-1_CREATED" src="https://github.com/user-attachments/assets/39e86132-7492-422e-bf29-04fb6e856e77" />

##### CLIENT-1 Basic Setup 2
<img width="1080" height="758" alt="CLIENT-1_CREATED2" src="https://github.com/user-attachments/assets/029cba0e-7f3a-4aff-a0c7-488546cab190" />

##### CLIENT-1 Networking
<img width="1328" height="797" alt="CLIENT-1_NETWORKING" src="https://github.com/user-attachments/assets/cf064453-69e9-4bda-bb44-e1faa8f47fc4" />

##### CLIENT-1 DNS
<img width="1205" height="740" alt="CLIENT-1_DNS_NETWORK_INTERFACE" src="https://github.com/user-attachments/assets/73e98b0a-fc95-42c0-a6b8-389696f1e65e" />

---
## 🥉 C — Configure Active Directory
### 1. Install AD DS on DC-1
Inside RDP session:
```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```
Promote to domain controller:
- New Forest
- Root Domain: lab.local

### AD DS Installation Wizard
#### DC-1 Sever Manager - Manage
<img width="1915" height="894" alt="DC-1_AddRolesAndFeatures_AD" src="https://github.com/user-attachments/assets/b97504a5-2521-4614-ae88-30bf272ec517" />

#### DC-1 Navigate to Add Roles and Features
<img width="247" height="165" alt="DC-1_AddRolesAndFeatures_AD1" src="https://github.com/user-attachments/assets/f16b8dec-dd2f-4262-ab56-1ada19393d3c" />

#### DC-1 Select server roles
<img width="777" height="562" alt="AD_DS_CHECKED" src="https://github.com/user-attachments/assets/f4e2d92a-613a-4879-a9e2-150e91b70d41" />

### DC-1 Add Roles and Features Wizard / Click Add Features
<img width="420" height="439" alt="AD_DS_ADD_FEATURES" src="https://github.com/user-attachments/assets/b3112f88-e879-4a0d-8b34-07abdef30bc6" />

### Install Comfirmation Page
<img width="809" height="581" alt="AD_DS_CONFIM_PAGE" src="https://github.com/user-attachments/assets/a67e3954-94a7-42f4-b570-d2cb7dc7bef7" />

### Installation Progress
<img width="810" height="587" alt="AD_DS_INSTALL_PROGRESS" src="https://github.com/user-attachments/assets/f377739b-ccbc-4ef2-a360-6c28187221be" />

### DC-1 Promotion
<img width="377" height="169" alt="DC-1_PROMOTION" src="https://github.com/user-attachments/assets/11c43f66-08c8-4f58-aa7c-ec14a0f2f0b4" />

### DC-1 Add a New Forest
<img width="758" height="563" alt="DC-1_PROMOTION_NEW_FOREST" src="https://github.com/user-attachments/assets/e625737d-348a-43ff-9305-b110a8766e00" />

### Check If Active Directory Tools Are Availabe
<img width="1915" height="768" alt="DC-1_ACTIVE_DIRECTORY_USERS_COMPUTERS" src="https://github.com/user-attachments/assets/96dff043-ed33-4eb0-883e-9b05c4664109" />

### 2. Verify DNS Functionality
- Run inside CLIENT-1:
```powershell
nslookup dc-1.lab.local
```
Expected result: resolves to 10.0.0.4
### Successful nslookup
### ping DC-1 and DC-1.lab.local
<img width="606" height="538" alt="CLIENT-1_CONNECTIVITY_CHECK1" src="https://github.com/user-attachments/assets/51d46ad9-60ee-40b5-ba4d-f0d9a479914c" />

### nslookup lab.local
<img width="485" height="181" alt="CLIENT-1_CONNECTIVITY_CHECK2" src="https://github.com/user-attachments/assets/885ba0c2-c716-4aaf-96a8-e6af65258286" />

---

## 🏁 D — Join Client to the Domain
### On CLIENT-1:
Settings → System → About → Rename this PC (Advanced)
→ Change → Domain → enter:
```powershell
lab.local
```
When prompted:
- Username: lab\Administrator
- Password: (your DC admin password)

📸 Successful Domain Join Message
#### Domain Join
<img width="497" height="326" alt="CLIENT-1_DOMAIN_JOIN" src="https://github.com/user-attachments/assets/60655de0-b057-4190-89c4-bf8733dea3e8" />

Restart CLIENT-1 → Log in
---
# 🌟 What I Learned
This project reinforced skills in:
- Azure virtual networking
- Identity & access management (IAM)
- Windows Server administration
- DNS design and configuration
- Active Directory domain architecture
- Troubleshooting connectivity & domain join issues
- Cloud documentation and diagramming






