# Step 2 – Install Active Directory Domain Services (AD DS) & DNS, and Join Clients

## 📌 Objective
Install Active Directory Domain Services (AD DS) with integrated DNS on Windows Server 2019, configure a new domain, and join Windows 10, Windows 11, and Debian clients to the domain.  
This forms the foundation for centralized user management, Group Policy application, and enterprise-grade administration.
---

## 🔹 Background
- **Active Directory (AD)** centralizes authentication, authorization, and management of users, computers, and resources.
- **DNS integration** with AD enables seamless name resolution within the domain.
- Joining clients to the domain allows centralized policy enforcement, easier management, and resource deployment.
- **Organizational Units (OUs)** logically group users and devices for targeted administration.

---

## 🛠️ Configuration Details
- **Domain Name:** corp.local  
- **Domain Controller (DC):** Windows Server 2019 (DC01)  
- **Static IP (DC01):** 192.168.10.10  
- **DNS Server:** 192.168.10.10  
- **Clients:** Windows 10, Windows 11, Debian  

---

## 🔹 Steps Performed

### 1️⃣ Install AD DS on Windows Server 2019
1. Open **Server Manager**.
  
2. In the top-right, click **Manage → Add Roles and Features**.

![Add_Roles_Features](images/1_Add_Roles.png)




3. **Before You Begin** → Click **Next**.
  
4. **Installation Type** → Select **Role-based or feature-based installation** → **Next**.\

![Role_Based](images/2_Role_Based.png)

5. **Server Selection** → Choose your server → **Next**.
![Choose_Server](images/3_Choose_Server.png)

6. **Server Roles** → Check **Active Directory Domain Services**.
![Active Directory](images/4_ActiveDirectory.png)

7. When prompted, click **Add Features** → **Next**.
![Add_Roles_Features](images/5_Add_Features.png)

8. Continue clicking **Next** until the **Confirmation** screen.
![Next](images/6_Next.png)
9. Click **Install** (do not close the wizard when finished).
![Install](images/7_Install.png)

10. After installation, click **Promote this server to a domain controller**.
![Promote_DC](images/8_Promote_DC.png)

---

### 2️⃣ Promote the Server to a Domain Controller
1. **Deployment Configuration** → Select **Add a new forest**.
2. **Root Domain Name:** `corp.local`
![Add_Forest](images/9_Add_Forest.png)

3. **Domain Controller Options:**
   - Keep **DNS Server** checked.
   - Set a **DSRM password** (Directory Services Restore Mode).
![Set_Pass](images/10_Set_Password.png)
![Next_DNS](images/11_Next_DNS.png)

4. **Additional Options:**
   - NetBIOS name will default to `CORP`.
![NetBIOS](images/12_NetBIOS.png)

5. **Paths:** Leave default locations.
![Default_Path](images/13_Default_Paths.png)

6. **Review & Install:** Click **Next** until the **Prerequisites Check** completes.
![Review_Next](images/14_Review_Next.png)

7. Click **Install** → Server will reboot automatically.
![Install](images/15_Install.png)
![Installation Complete](images/16_Installation_Complete.png)

---
### 3️⃣ Authorize DHCP in Active Directory
- In AD-integrated networks, **only authorized DHCP servers** can issue IP leases.  
- Without authorization, clients may fail to receive IPs.

1. Open **DHCP Management Console** (Server Manager → Tools → DHCP).  
![Tools_DHCP](images/17_Tools_DHCP.png)

2. Right-click your server (e.g., `WINSERVER.corp.local`) → **Authorize**.  
![Authorize_DHCP](images/18_Authorize_DHCP.png)

3. Wait until the **red arrow turns green** → DHCP is active.
![Verify_DHCP](images/19_Verify_DHCP.png)

---

### 4️⃣ Join Clients to the Domain

#### 🖥️ Windows 10 / 11
1. Ensure **Preferred DNS** is set to DC (`192.168.10.10`).  
2. **Right-click Start → System → Advanced system settings**.  
3. Under **Computer Name**, click **Change**.  
4. Select **Domain**, enter: `corp.local`.  
5. Enter domain **Administrator** credentials.  
6. Accept welcome prompt → Reboot.  

---

#### 🐧 Debian Client
```bash
# Set DNS server to Domain Controller
sudo nano /etc/resolv.conf
# Add:
nameserver 192.168.10.10

# Install required packages
sudo apt update && sudo apt install realmd sssd samba-common \
    samba-common-bin packagekit adcli -y

# Discover the domain
realm discover corp.local

# Join the domain
sudo realm join corp.local -U Administrator

# Verify
realm list










