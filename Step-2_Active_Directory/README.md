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

6. **Server Selection** → Choose your server → **Next**.
![Choose_Server](images/3_Choose_Server.png)

7. **Server Roles** → Check **Active Directory Domain Services**.
![Active Directory](images/4_ActiveDirectory.png)

8. When prompted, click **Add Features** → **Next**.
![Add_Roles_Features](images/5_Add_Features.png)

9. Continue clicking **Next** until the **Confirmation** screen.
![Next](images/6_Next.png)
10. Click **Install** (do not close the wizard when finished).
![Install](images/7_Install.png)

11. After installation, click **Promote this server to a domain controller**.
![Promote_DC](images/8_Promote_DC.png)

---

### 2️⃣ Promote the Server to a Domain Controller
1. **Deployment Configuration** → Select **Add a new forest**.
2. **Root Domain Name:** `corp.local`
![Add_Roles_Features](images/_Add_Roles.png)

4. **Domain Controller Options:**
   - Keep **DNS Server** checked.
   - Set a **DSRM password** (Directory Services Restore Mode).
  ![Add_Roles_Features](images/_Add_Roles.png)

5. **Additional Options:**
   - NetBIOS name will default to `CORP`.
6. **Paths:** Leave default locations.
7. **Review & Install:** Click **Next** until the **Prerequisites Check** completes.
8. Click **Install** → Server will reboot automatically.

---

### 3️⃣ Join Clients to the Domain

#### **Windows 10 / 11 Clients**
1. Set the **Preferred DNS** server to the DC IP (`192.168.10.10`) in the adapter properties.
2. **Right-click Start → System → Advanced system settings**.
3. Under **Computer Name**, click **Change**.
4. Select **Domain** and enter: `corp.local`.
5. Enter domain admin credentials.
6. Accept the welcome prompt → Reboot.


#### **Debian Client**
```bash
# Set DNS server to domain controller
sudo nano /etc/resolv.conf
# Add:
nameserver 192.168.10.10

# Install realmd & samba packages
sudo apt update && sudo apt install realmd sssd samba-common packagekit samba-common-bin adcli -y

# Discover the domain
realm discover corp.local

# Join the domain
sudo realm join corp.local -U Administrator

# Verify
realm list










