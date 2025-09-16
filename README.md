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

## 🗂️ [Step-2: Active Directory Setup](./Step-2_Active_Directory)  

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


## 🏢 [Step-3: Organize Active Directory](./Step-3_Organize_AD)
  
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


## ⚙️ [Step-4: Advanced Group Policies](./Step-4_Advanced_GPOs)

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


## 📦 [Step-5: Quotas & File Screening](./Step-5_Quotas_&_File_Screening)  

# Step 5 – Quotas & File Screening (FSRM) 🗄️🔒

**Objective:**  
Use File Server Resource Manager (FSRM) to manage storage on file server: enforce quotas, block unwanted file types, and generate notifications.

---

## 1️⃣ Install File Server Resource Manager

- Server Manager → Manage → Add Roles and Features  
- Role-based installation → File and iSCSI Services → **File Server Resource Manager**  
- Complete installation and verify

---

## 2️⃣ Configure Quotas

- Open FSRM → **Quota Management → Create Quota**  
- Target folder: `D:\Shares\HRData$`  
- **Quota Type:** Hard Quota  
- **Size Limit:** Example 500 MB – 1 GB  
- Optional: **Notification Thresholds** (e.g., 80%) → Email, Event Log, Command, Report  
- Save template: `HRData_Quota`  
- Verify quota is created

---

## 3️⃣ Configure File Screening

- Open FSRM → **File Screening Management → Create File Screen**  
- Target folder: `D:\Shares\HRData$`  
- **Active Screening** → Block specific file types:  
  - Audio/Video, Executables, Compressed files, Images, Web files  
- Optional: Save as **Custom Template**  
- Verify file screen is applied

---

## 4️⃣ Verification

### Test Quota

- On HR client (EveHR, mapped drive `Z:`):
  ```powershell
  fsutil file createnew Z:\test1.dat 52428800  # 50 MB
  fsutil file createnew Z:\test2.dat 62914560  # 60 MB → should fail

---

## 4. Verification ✅

### 🔹 Confirm Quota Exceeded
- On a client (e.g., EveHR – Win10), attempt to create files that exceed the quota limit.  
- You should see an **error message** such as “Not enough disk space.”  
- On the server, open **FSRM** to confirm the **quota usage percentage** is updated.

---

### 🔹 Test File Screening
- Attempt to save blocked file types (e.g., `test.exe`, `.mp3`) into the HR share.  
- The operation should be **denied**.  
- Verify in FSRM that the **file screen rules** were enforced.

---

### 🔹 Check Quota Alerts
- Open **Event Viewer** → navigate to:  
  `Custom Views → Administrative Events`  
- Look for **Warning Events** triggered by FSRM when quota thresholds were exceeded.  
- Example:  
  `User CORP\EveHR has exceeded the 80% quota threshold for the quota on C:\Shares\HRData$`

---

## 📌 Notes
- **Centralized Storage Management** → FSRM enforces storage policies across the enterprise.  
- **Quotas** → Prevent users or departments from consuming excessive disk space.  
- **File Screening** → Blocks unapproved file types to maintain compliance and reduce risks.  
- **Notifications** → Alerts allow admins to proactively monitor and respond before issues escalate.  

✅ At this point, **FSRM is fully configured** to control disk usage, block risky files, and notify admins of quota violations.

---
## 🔐 [Step-6: Security Policies](./Step-6_Security_Policies)  

**Objective:**  
In this step, we configure domain-wide security policies to enforce strong passwords, account lockouts, and role-based access control. We also implement fine-grained password policies (FGPP) for different groups.

---

## 1. Password Policy Configuration

### Steps
1. Open **Group Policy Management Console (GPMC)**.  
2. Navigate to **Group Policy Objects → Default Domain Policy → Edit**.  
3. Go to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`

### Configured Settings
- **Enforce Password History:** 24 passwords remembered  
- **Maximum Password Age:** 60 days  
- **Minimum Password Age:** 1 day  
- **Minimum Password Length:** 12 characters  
- **Password Must Meet Complexity Requirements:** Enabled  

### Testing & Validation
- Forced a password reset for `AliceIT` in **ADUC**.  
- Attempted weak password → **Rejected**  
- Attempted strong password → **Accepted**

✅ Outcome: All users in **corp.local** must follow strong password standards.

---

## 2. Account Lockout Policy

### Steps
1. Open **GPMC** → Default Domain Policy → Edit  
2. Navigate to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy`

### Configured Settings
- **Account Lockout Threshold:** 3 invalid logon attempts  
- **Account Lockout Duration:** 30 minutes  
- **Allow Administrator Account Lockout:** Disabled  
- **Reset Account Lockout Counter After:** 30 minutes

### Testing & Validation
- Attempted login with wrong password 3x → **Account locked**  
- Confirmed lockout message and automatic unlock after 30 minutes

✅ Outcome: Protects against brute-force attacks with temporary lockouts.

---

## 3. User Rights Assignment (RBAC)

### Steps
1. Open **GPMC → Default Domain Policy → Edit**  
2. Navigate to:  
   `Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → User Rights Assignment`

### Configured Settings
#### Deny Log on Locally
- Applied to `HR_Staff`  
- Prevents HR users from logging on to servers or domain controllers

#### Allow Log on Through Remote Desktop Services
- Applied to `IT_Staff`  
- Restricts remote desktop access to IT administrators

### Testing & Validation
- HR staff → local login and RDP **denied**  
- IT staff → RDP **granted**

✅ Outcome: Enforces least privilege; only authorized users can perform administrative tasks.

---

## 4. Fine-Grained Password Policies (FGPP)

### Scenario
- Admins require stricter password rules than standard users  
- Implemented via **Password Settings Objects (PSOs)** in ADAC

### Configuration
#### Admin Accounts (IT_Staff)
- Minimum Password Length: 15  
- Enforce Password History: 5  
- Complexity: Enabled

#### Standard Users (HR_Staff)
- Minimum Password Length: 10  
- Enforce Password History: 5  
- Complexity: Enabled

### Testing & Validation
- Attempted passwords below requirements → **Rejected**  
- Passwords meeting requirements → **Accepted**  
- Verified via ADAC and PowerShell:
```
Get-ADUserResultantPasswordPolicy -Identity AliceIT
Get-ADUserResultantPasswordPolicy -Identity EveHR
```

## 👤 [Step-7: Service Accounts](./Step-7_Service_Accounts)  

**Objective:**  
Set up a single-purpose workstation to automatically log in with a **service account**, launch a specific web page in full-screen mode, and restrict local logon for standard users. Simulates a kiosk-style setup without using Windows Kiosk mode.

**Prerequisites**
- Windows Server with AD DS installed  
- Windows 10 Pro / Enterprise client  
- Sysinternals Suite  
- AD security groups (e.g., `LabUsers`)  
- Web browser (e.g., Chrome)  

---

## 1. Create a Service Account in Active Directory
1. Open **ADUC** → optionally create `ServiceAccounts` OU.  
2. Create a new user:  
   - **Name:** `Website_FirstName LastName Login`  
   - **User logon name:** `$Website-Login`  
   - **Password settings:**  
     - Uncheck “User must change password at next logon”  
     - Check “User cannot change password”  
     - Check “Password never expires”  
3. Add a description: *“Auto-login to display websites”*

✅ Outcome: Service account created and ready for auto-login.

---

## 2. Install Sysinternals Suite on Client
1. Download and extract **Sysinternals Suite** to a network-accessible folder.  
2. Launch **Autologon64.exe**.

---

## 3. Configure Auto-Login for the Service Account
1. Open **Autologon64** → enter:  
   - Username: `$Website-Login`  
   - Domain: `corp.local`  
   - Password: [service account password]  
2. Click **Enable** → reboot client.

✅ Outcome: Windows logs in automatically with the service account.

---

## 4. Set Browser to Auto-Start with Specific Web Page
1. Install Chrome → Settings → On startup → Open a specific page → Enter URL.  
2. Modify Chrome shortcut → append `--start-Fullscreen` → copy shortcut to `shell:startup`.

✅ Outcome: Browser opens automatically in full-screen showing the desired webpage.

---

## 5. Configure System Settings
- Prevent sleep: Settings → Power & Sleep → Sleep → Never.  
- Test reboot → Chrome should auto-launch in full-screen.

---

## 6. Restrict Local Logon Using Group Policy
1. Open **GPMC** → create new GPO: `Restrict Logon to Service Account`.  
2. Edit → Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → User Rights Assignment.  
3. Configure **Deny log on locally** → add `AllEmployees` (standard users).  
4. Link GPO to OU containing kiosk client → run `gpupdate /force`.

✅ Outcome: Only service account can log in locally; standard users are denied.

---

## 7. Verification
- Reboot client → auto-login with service account.  
- Chrome opens full-screen with the configured page.  
- Attempt login with standard user → access denied.

---

## Notes
- **Service Account:** Dedicated for kiosk use, prevents password expiry and changes.  
- **Auto-Login:** Ensures workstation starts without manual intervention.  
- **Full-Screen Browser:** Displays desired content automatically.  
- **Restricted Logon:** Enhances security by preventing unauthorized local access.
  
## 📁 [Step-8: Effective Permissions & Inheritance](./Step-8_Advanced_Windows_File_Sharing)
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
