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

- 🖧 [Step-1: DHCP on Windows Server 2019](./Step-1_DHCP_Win2019)  
- 🖥️ Assigned a **static IP** to Windows Server 2019 for VLAN 10  
- 🔒 Disabled **pfSense DHCP service** on VLAN 10  
- 📦 Installed & authorized the **DHCP Server role** in Active Directory  
- 📑 Created & activated a **DHCP Scope**  
  - Scope Name: `VLAN10-Scope`  
  - Range: `192.168.10.50 – 192.168.10.200`  
  - Exclusions: `192.168.10.1 – 192.168.10.49`  
  - Default Gateway: `192.168.10.1`  
  - DNS: `192.168.10.10, 192.168.20.2`  
  - Lease Duration: **8 Days**  
- 🧪 Verified DHCP leases on:  
  - Windows 10 client (`ipconfig /renew`)  
  - Windows 11 client (`ipconfig /renew`)  
  - Debian client (`dhclient`)  

- 🗂️ [Step-2: Active Directory Setup](./Step-2_Active_Directory)  

**Objective:**  
Install AD DS with integrated DNS on Windows Server 2019, configure a new domain (`corp.local`), and join Windows 10, Windows 11, and Debian clients.

**Key Tasks / Highlights:**  
- 🖥️ **Install AD DS Role** on Windows Server 2019  
  - Add Roles & Features → AD DS  
  - Promote server to **Domain Controller**  
  - Configure new forest/domain (`corp.local`)  
  - Set DSRM password for recovery  

- 🌐 **Configure DNS**  
  - Integrated with AD for seamless name resolution  
  - Configure **Forwarders** for external internet access  

- 🔐 **Secure Domain Controller**  
  - Rename default Administrator to `SecAdmin01`  
  - Confirm login with new credentials  
  - Reduce attack surface / brute-force risk  

- 🖥️ **Join Clients to Domain**  
  - Windows 10 / 11 clients  
    - Set preferred DNS to DC (`192.168.10.10`)  
    - Change computer name (AliceIT, EveHR)  
    - Join domain and reboot  
  - Debian client  
    - Configure DNS  
    - Install required packages (realmd, sssd, adcli, samba-common)  
    - Discover domain & join using `realm join corp.local -U Administrator`  

- 🔍 **Verify DNS Forward & Reverse Lookups**  
  - Confirm clients resolve correctly to IPs  
  - Ensure PTR records map IP → hostname  

**Outcome:**  
- Centralized authentication & authorization for all clients  
- AD-integrated DNS functioning properly for forward & reverse lookups  
- Clients successfully joined to domain  
- Enhanced security by renaming default admin account


- 🏢 [Step-3: Organize Active Directory](./Step-3_Organize_AD)
  
**Objective:**  
Structure Active Directory with OUs, user accounts, and security groups for centralized administration, policy enforcement, and role-based access control.

**Key Tasks / Highlights:**  

- 🏢 **Create Organizational Units (OUs)**  
  - `LabUsers` → `IT`, `HR`  
  - `LabComputers` → `Workstations`, `Servers`  
  - `LabGroups` for grouping users logically  

- 👤 **Create User Accounts**  
  - Users for IT: `Alice IT`, `Bob IT`  
  - Users for HR: `Eve HR`, `John Doe`, `Charlie HR`  
  - Set initial passwords and enforce change at first logon  

- 🔒 **Create Security Groups & Add Users**  
  - `IT_Staff` → IT OU users  
  - `HR_Staff` → HR OU users  
  - Assign users to respective groups for role-based permissions  

- 📜 **Create and Link Group Policies (GPOs)**  
  - IT OU → `IT_User_Policy` (e.g., desktop wallpaper, disable Control Panel)  
  - HR OU → `HR_User_Policy` (e.g., folder redirection, password-protected screensaver)  
  - Link GPOs to OUs and verify application  

- ✅ **Verify Setup**  
  - Windows 10 / 11 clients: log in, refresh policies (`gpupdate /force`)  
  - Debian client: verify domain join and DNS (`realm list`, `nslookup`)  
  - Confirm OU hierarchy, user accounts, groups, and GPO links  

**Outcome:**  
- AD is structured for logical management.  
- Users and groups organized for simplified permission management.  
- GPOs applied to enforce policies per OU.  
- Lab environment ready for file server permissions, remote management, and further security configurations.


- ⚙️ [Step-4: Advanced Group Policies](./Step-4_Advanced_GPOs)

**Objective:**  
Apply realistic department-specific policies to IT and HR OUs to enforce security, usability, and compliance in a lab environment.

---

**IT Department (OU: LabUsers → IT)**  
**GPO Name:** `IT_User_Policy` | **Security Group:** `IT_Staff`  

- 🖼️ **Desktop Wallpaper**  
  - Store image in shared folder: `C:\Wallpapers` → Share with `IT_Staff`  
  - UNC path: `\\WIN-Server\Wallpapers\ITBackground.jpg`  
  - Configure via GPO: `User Configuration → Policies → Administrative Templates → Desktop → Desktop Wallpaper`  

- 🚫 **Disable Control Panel Access**  
  - GPO Path: `User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel`  
  - Prevents unauthorized changes for IT staff  

- ✅ **Verification**  
  - Log in as IT user (AliceIT) → `gpupdate /force` → Confirm wallpaper and restricted Control Panel  

---

**HR Department (OU: LabUsers → HR)**  
**GPO Name:** `HR_User_Policy` | **Security Group:** `HR_Staff`  

- 📂 **Folder Redirection**  
  - Documents redirected to: `\\WIN-SERVER\FOLDERREDIR$`  
  - Per-user folders automatically created on server  
  - GPO Path: `User Configuration → Policies → Windows Settings → Folder Redirection → Documents`  
  - Optional: disable exclusive rights in lab environments  

- 🔒 **Password-Protected Screensaver**  
  - Timeout: 2–5 minutes (configurable)  
  - GPO Path: `User Configuration → Administrative Templates → Control Panel → Personalization`  

- 🛡️ **USB Storage Restriction**  
  - Deny read/write on removable drives  
  - GPO Path: `User Configuration → Administrative Templates → System → Removable Storage Access`  

- 📁 **HR Shared Folder (`HRData$`)**  
  - Create on server with NTFS & share permissions for `HR_Staff`  
  - Optional mapping via GPO → Z: drive for HR users  

- ✅ **Verification**  
  - Log in as HR user (EveHR) → `gpupdate /force`  
  - Check folder redirection, screensaver, USB restriction, and access to HR shared folder  

---

**Notes:**  
- IT policies focus on system control and branding  
- HR policies enforce data protection and compliance  
- Lab-ready configuration allows testing of realistic enterprise policies  
- Further expansion possible: software deployment, security hardening, compliance enforcement


- 📦 [Step-5: Quotas & File Screening](./Step-5_Quotas_&_File_Screening)  
  *Implemented FSRM quotas, file screening for storage governance*  

- 🔐 [Step-6: Security Policies](./Step-6_Security_Policies)  
  *Configured account lockout, auditing, and password rules*  

- 👤 [Step-7: Service Accounts](./Step-7_Service_Accounts)  
  *Created and delegated least-privilege service accounts*
  
- 📁 [Step-8: Effective Permissions & Inheritance](./Step-8_Advanced_Windows_File_Sharing)
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

---

## 📇 Contact

🔗 [LinkedIn: John Slaughter](https://www.linkedin.com/in/john-slaughter-08a872262/)
