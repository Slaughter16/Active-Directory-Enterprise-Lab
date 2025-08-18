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

![AD_Tools](images/1_AD_Users_Computers.png)

2. Right-click your domain (`corp.local`) → **New → Organizational Unit**.

![Create_OrgUnit](images/2_Org_Unit.png)


3. Enter the OU name (e.g., `LabUsers`) → **OK**
![LabUsers_OU](images/3_LabUsers.png)

4. Repeat to create sub-OUs like 'LabUsers →`IT` & `HR`, `LabComputers → Workstations` & `Servers`. `LabGroups`.

![IT_LabUsers](images/4_IT_LabUsers.png)

![HR_LabUsers](images/5_HR_LabUsers.png)
 
![Lab_Computers_OU](images/6_Lab_Computers_.png)

![LabComp_Workstations](images/7_LabComp_Workstations.png)

![LabComp_Servers](images/8_LabComp_Servers.png)

![LabGroup_OU](images/9_LabGroups.png)


5. Verify AD Users and Computers

![Verify_AD_OU](images/10_Verify_OU.png)


---

### 2️⃣ Create User Accounts

1. In **Active Directory Users and Computers**, expand your domain (`corp.local`) and navigate to the **OU where you want the user** (e.g., `LabUsers → IT` or `LabUsers → HR`) We chose HR this time.

![OU_Select](images/11_Select_OU.png)
   
2. Right-click the chosen OU (HR) → **New → User**.  
   
![New_User](images/12_New_User.png)

   
3. Enter user details:  
   - **First Name:** John  
   - **Last Name:** Doe  
   - **User Logon Name (UPN):** `JdoeHR@corp.local`
   - **User Logon Name (Pre-Windows 2000):** `CORP\JdoeHR`
   - **Click Next** 

![User_Details](images/13_Jdoe.png)

 
4. Set an **initial password**.  
   - Check **User must change password at next logon** (recommended for lab security).  
   - Optionally, uncheck if you want static passwords.

![Set_Password_Jdoe](images/14_Set_Password_Jdoe.png)

5. Review and Verify creation of John Doe user in HR
![Review_Jdoe_Users](images/15_Review_Jdoe_User.png)
![Verify_Users](images/16_Verify_Jdoe_HR.png)

6. Repeat for multiple accounts:

   - `HR` OU → Create users like `Eve HR`, `Charlie HR`
 
![New_User_EveHR](images/17_New_User_EveHR.png)
![New_User_CharlieHR](images/18_New_User_CharlieHR.png)

   
   - `IT` OU → Create users like `Alice IT`, `Bob IT`  

![New_User_AliceIT](images/19_New_User_AliceIT.png)
![New_User_BobIT](images/20_New_User_BobIT.png)

    Example usernames:
   
   - `eve.hr@corp.local`  
   - `charlie.hr@corp.local`
   - `alice.it@corp.local`  
   - `bob.it@corp.local`  
  
    
7. Verify that the accounts appear inside their respective OUs.  

-HR List
   ![Verify_Users_HR](images/21_Verify_Users_HR.png)

-IT List
   ![Verify_Users_IT](images/22_Verify_Users_IT.png)

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
