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

2. [Step-2_Active_Directory](./Step-2_Active_Directory)  
   - Install and configure AD DS, promote Domain Controller  

3. [Step-3_Organize_AD](./Step-3_Organize_AD)  
   - Create Organizational Units (OUs), users, and security groups  

4. [Step-4_Advanced_GPOs](./Step-4_Advanced_GPOs)  
   - Apply advanced Group Policy Objects (password policies, drive mappings, restrictions)  

5. [Step-5_Quotas_&_File_Screening](./Step-5_Quotas_&_File_Screening)  
   - Implement quotas and file screening with FSRM  

6. [Step-6_Security_Policies](./Step-6_Security_Policies)  
   - Configure account lockout, auditing, and password rules  

7. [Step-7_Service_Accounts](./Step-7_Service_Accounts)  
   - Create and delegate least-privilege service accounts  

8. [Step-8_Effective_Permissions_and_Inheritance](./Step-8_Effective_Permissions_and_Inheritance)  
   - Configure NTFS/share permissions with:  
     - Inheritance vs explicit permissions  
     - Group-based access (Project group)  
     - Separate share for Contracts folder (selective access)  
     - Explicit deny for sensitive Confidential folder  
     - Tested access across multiple users (Alice, EveHR)  

---

---

## 🚀 Skills Demonstrated  
- **Active Directory Administration**: OU design, user/group management, service accounts  
- **Group Policy Management**: enforcing security baselines, drive mapping, desktop restrictions  
- **DHCP/DNS Configuration**: centralized IP management, client integration  
- **File & Storage Management**: FSRM quotas, file screening, NTFS vs share permissions  
- **Access Control**: inheritance, explicit allow/deny, least-privilege delegation  
- **Auditing & Compliance**: security policies, access monitoring, account lockout policies  
- **Cross-platform IT Integration**: Windows & Linux clients in AD domain  
- **Virtualization**: Enterprise lab environment hosted on Proxmox

---

## 📈 Next Steps (Planned Enhancements)  
- Implement **centralized logging & auditing** with Windows Event Forwarding / SIEM integration  
- Configure **offline files & folder redirection** for users  
- Automate **user and group provisioning** with PowerShell scripts  
- Demonstrate **printer sharing** with AD permissions  
- Explore **Microsoft Intune / Remote Management** integration

---

## ✅ Outcome  
By the end of this lab:  
- Built a fully functional **enterprise IT environment**  
- Demonstrated **end-to-end system administration** across networking, identity, storage, and security  
- Validated **real-world enterprise practices** in Active Directory and Windows Server management  

This project serves as a portfolio-ready showcase of **enterprise IT and cybersecurity skills**.  
