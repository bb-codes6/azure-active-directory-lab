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

## 🧩 Architecture Diagram
Azure Resource Group (azure-lab)
│
├── Virtual Network (AD_VNet)
│     └── Subnet: default (10.0.0.0/25)
│
├── DC-1  (Windows Server 2022)
│     ├── Private IP: 10.0.0.4 (Static)
│     ├── AD DS + DNS Server
│     └── Domain: lab.local
│
└── CLIENT-1 (Windows 10/11)
      ├── Private IP: 10.0.0.5 (Dynamic)
      └── Joined to lab.local
---
# Step-by-Step Deployment Guide
---


