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

## 🖧 [Step-1: DHCP on Windows Server 2019](./Step-1_DHCP_Win2019)  

**Objective:**  
Configure DHCP to provide IP addressing for VLAN 10 clients.

**Key Skills & Outcomes:**  
- Assigned **static IP** to Windows Server for VLAN 10  
- Disabled pfSense DHCP to avoid conflicts  
- Installed & authorized **DHCP Server role** in Active Directory  
- Created/activated **VLAN10-Scope**: `192.168.10.50–192.168.10.200` with exclusions, gateway, DNS, and 8-day lease  
- Verified DHCP leases on Windows 10/11 and Debian clients  

✅ Demonstrates **DHCP deployment, scope management, and cross-platform client verification**

---

## 🗂️ [Step-2: Active Directory Setup](./Step-2_Active_Directory)  

**Objective:**  
Install AD DS with integrated DNS, create domain `corp.local`, and join Windows & Debian clients.

**Key Skills & Outcomes:**  
- Installed AD DS on Windows Server 2019 and promoted to **Domain Controller**  
- Configured AD-integrated DNS with forwarders for external resolution  
- Secured DC by renaming default admin (`SecAdmin01`)  
- Joined clients: Windows 10/11 (AliceIT, EveHR) and Debian (realmd + sssd)  
- Verified forward/reverse DNS resolution for all clients  

✅ Demonstrates **domain deployment, DNS integration, client joining, and security best practices**

---

## 🏢 [Step-3: Organize Active Directory](./Step-3_Organize_AD)

**Objective:**  
Structure AD with OUs, users, and security groups for centralized management and role-based access.

**Key Skills & Outcomes:**  
- Created OUs: `LabUsers` (IT, HR), `LabComputers` (Workstations, Servers), `LabGroups`  
- Added users: IT (`Alice IT`, `Bob IT`), HR (`Eve HR`, `John Doe`, `Charlie HR`)  
- Created security groups (`IT_Staff`, `HR_Staff`) and assigned users  
- Linked department-specific GPOs (`IT_User_Policy`, `HR_User_Policy`)  
- Verified on Windows & Debian clients (`gpupdate /force`, `realm list`, `nslookup`)  

✅ Demonstrates **AD structuring, user/group management, and GPO deployment**

---

## ⚙️ [Step-4: Advanced Group Policies](./Step-4_Advanced_GPOs)

**Objective:**  
Implement department-specific policies for IT and HR to enforce security, compliance, and usability.

**Key Skills & Outcomes:**  
- **IT OU (`LabUsers → IT`)**
  - Desktop wallpaper via GPO  
  - Disabled Control Panel access for standardization & security  
- **HR OU (`LabUsers → HR`)**
  - Folder redirection to server (`HRData$`)  
  - Password-protected screensaver  
  - USB storage restrictions
  - Shared folder setup
  - Map network drive via GPO and Manually 
- Verified policy application on clients (`gpupdate /force`)  

✅ Demonstrates **GPO design, OU-specific policies, and enterprise-level security enforcem

---

## 📦 [Step-5: Quotas & File Screening](./Step-5_Quotas_&_File_Screening)  

**Objective:**  
Implement **FSRM** to manage storage, enforce quotas, block unwanted files, and generate alerts.

**Key Skills & Outcomes:**  
- Configured **hard quotas** on departmental shares to prevent excessive disk usage  
- Implemented **file screening** to block unapproved file types (executables, media, compressed files)  
- Set up **alerts and notifications** to monitor quota usage  
- Verified enforcement: users blocked from exceeding quotas or saving restricted files  

✅ Demonstrates **centralized storage management, compliance enforcement, and proactive IT administration**

---

## 🔐 [Step-6: Security Policies](./Step-6_Security_Policies)  

**Objective:**  
Enforce **strong password standards, account lockouts, role-based access control (RBAC),** and **fine-grained password policies** for different user groups.

**Key Skills & Outcomes:**  
- Configured **password policies**: minimum length, complexity, history, and expiration  
- Implemented **account lockout** to prevent brute-force attacks  
- Applied **RBAC**: restrict local logins for HR staff, allow RDP for IT admins  
- Set **Fine-Grained Password Policies (FGPP)**: stricter rules for IT admins vs standard users  
- Verified policies using ADUC, ADAC, and PowerShell  

✅ Demonstrates **enterprise-level security configuration, compliance, and least-privilege enforcement**

---

## 👤 [Step-7: Service Accounts](./Step-7_Service_Accounts)  

**Objective:**  
Configure a **dedicated service account** to auto-login a workstation, launch a web page in full-screen, and restrict standard user access—simulating a kiosk setup.

**Key Skills & Outcomes:**  
- Created **service account** in AD with non-expiring password  
- Configured **auto-login** on Windows client using Sysinternals Autologon  
- Set browser to **auto-start in full-screen** with specified webpage  
- Applied **GPO restrictions** to deny local logon for standard users  
- Verified auto-login, full-screen launch, and restricted access  

✅ Demonstrates **service account management, kiosk automation, and secure access control**

---

## 📁 [Step-8: Effective Permissions & Inheritance](./Step-8_Advanced_Windows_File_Sharing)

**Objective:**  
Demonstrate **NTFS permissions, inheritance, group-based access, and explicit deny** using shared folders.

**Key Skills & Outcomes:**  
- Created **Project security group** in AD and assigned users  
- Set up **folder structure**: Common (everyone), Project (restricted), Events (all)  
- Configured **share and NTFS permissions** with inheritance  
- Restricted **Project folder** to group members, applied **explicit deny** for sensitive subfolders  
- Created **Contracts subfolder** with selective access for specific users  
- Verified access: members vs non-members  

✅ Demonstrates **granular access control, group-based permissions, and enterprise-level file security**

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

---

## 📇 Contact

🔗 [LinkedIn: John Slaughter](https://www.linkedin.com/in/john-slaughter-08a872262/)
