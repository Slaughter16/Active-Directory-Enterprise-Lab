# 🏢 Enterprise IT Infrastructure Lab – Active Directory, Group Policy, Security

## 📌 Overview  
This project simulates a **real-world enterprise IT environment** hosted on a **Proxmox mini-PC**.  
It includes a **Windows Server 2019 Domain Controller**, **Windows 10/11 clients**, and a **Debian Linux client**, all integrated into an Active Directory domain.  


The lab demonstrates core IT administration skills:  
- **Identity & Access Management** with Active Directory  
- **Networking Services** (DHCP, DNS)  
- **Group Policy Objects (GPOs)** for centralized configuration  
- **File Server Management** with NTFS permissions, quotas, file screening  
- **Security & Compliance** (auditing, service accounts, explicit deny permissions)  
- **Cross-platform integration** (Windows & Linux clients in the domain)  

This environment replicates **enterprise-style system administration**, suitable for showcasing on a resume or in job interviews.  

---


## ⚙️ Technology Stack  
- **Hypervisor:** Proxmox VE (Mini PC host)  
- **Domain Controller:** Windows Server 2019  
- **Clients:** Windows 10, Windows 11, Debian Linux  
- **Services Implemented:** Active Directory, DHCP, DNS, Group Policy, File Server Resource Manager (FSRM), NTFS Permissions, Auditing

---

## 📚 Table of Contents  

1. [Step-1_DHCP_Win2019](./Step-1_DHCP_Win2019)  
   - Configure DHCP server and scopes for AD clients  
