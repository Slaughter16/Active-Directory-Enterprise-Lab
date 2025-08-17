# Step 3 – Organize Active Directory, Create Users & Groups

## 📌 Objective
Organize Active Directory by creating Organizational Units (OUs), user accounts, and security groups.  
This enables structured administration, targeted Group Policy application, and role-based access management in your lab environment.

---

## 🔹 Background
- **Organizational Units (OUs):** Logical containers in AD to group users, computers, and groups.  
- **User Accounts:** Individual identities in AD for authentication and access control.  
- **Security Groups:** Collections of users for simplified permissions management.  
- **Group Policy Objects (GPOs):** Rules that can be applied to OUs to enforce policies.

---

## 🛠️ Configuration Details
- **Domain Name:** corp.local  
- **Domain Controller:** Windows Server 2019 (DC01)  
- **Static IP (DC01):** 192.168.10.10  
- **Clients:** Windows 10, Windows 11, Debian  
- **Example OU Structure:**  

---

## 🔹 Steps Performed

### 1️⃣ Create Organizational Units (OUs)
1. Open **Server Manager → Tools → Active Directory Users and Computers**.  
2. Right-click your domain (`corp.local`) → **New → Organizational Unit**.  
3. Enter the OU name (e.g., `Users`) → **OK**.  
4. Repeat to create sub-OUs like `IT`, `HR`, `Computers → Workstations`, `Servers`, `Groups`.

![Create_OU](images/1_Create_OU.png)

---

### 2️⃣ Create User Accounts
1. Navigate to the appropriate OU (e.g., `Users → IT`).  
2. Right-click → **New → User**.  
3. Fill in **First Name, Last Name, User Logon Name** (e.g., `jlee`).  
4. Set initial password.  
5. Optionally check **User must change password at next logon**.  
6. Click **Next → Finish**.

![Create_User](images/2_Create_User.png)


---

### 3️⃣ Create Security Groups
1. Navigate to **Groups** OU.  
2. Right-click → **New → Group**.  
3. Name the group (e.g., `ITAdmins`).  
4. Select **Group scope:** Global, **Group type:** Security → **OK**.  
5. Add members (users) to the group by right-clicking the group → **Properties → Members → Add**.

![Create_Group](images/3_Create_Group.png)

---

### 4️⃣ Apply Basic Group Policy Objects (GPOs)
1. Open **Server Manager → Tools → Group Policy Management**.  
2. Right-click your OU (e.g., `Workstations`) → **Create a GPO in this domain, and Link it here**.  
3. Name the GPO (e.g., `Desktop Security Policy`).  
4. Edit GPO → Configure policies such as:
   - **Password Policies:** Minimum length, complexity, account lockout.  
   - **Desktop Restrictions:** Control Panel, drives, or software restrictions.  
   - **Scripts:** Login or logoff scripts if needed.  

![GPO_Example](images/4_GPO_Example.png)

---

### 5️⃣ Test Users and Groups
1. Log in from a client machine (Win 10/11 or Debian) using a domain user account.  
2. Confirm policies apply, and access permissions match the user’s group membership.  
3. Verify OU hierarchy and GPO links in **Active Directory Users and Computers** and **Group Policy Management Console**.

---

✅ **Outcome:**  
- Active Directory is structured with OUs for logical management.  
- Users and groups are created and organized.  
- GPOs applied to target OUs for policy enforcement.  
- Lab is ready for file server permissions, remote tools, and additional security configurations.
